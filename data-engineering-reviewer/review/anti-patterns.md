# Anti-patterns

Scan for these before the pillar review. Each one is a pattern a senior reviewer would not let reach production. Severity is the default; adjust it with [severity.md](severity.md) if the context lowers the risk.

Patterns are written for any orchestrator; examples in parentheses use Airflow terms. Translate them with [tool-mappings.md](tool-mappings.md). dbt-specific patterns are in that file.

## Contents
- Failure handling
- Time and determinism
- Loads and transactions
- Secrets and access
- Orchestrator misuse
- Data quality and testing
- Integration

## Failure handling

| Pattern | Why it is dangerous | Default severity | Pillar |
|---|---|---|---|
| `except: pass`, `except Exception: log(...)` with no re-raise | The task succeeds while the data is wrong or missing. The orchestrator reports green; nobody is alerted. | Critical | A2 |
| Returning `None`, an empty DataFrame or a status string instead of raising | Downstream tasks run on empty or partial input and overwrite good data. | Critical | A2 |
| `sys.exit(0)` or an early `return` on an error path | Same as above: success state on failure. | Critical | A2 |
| Retries on a task that appends | Each retry duplicates rows. | Critical | A1 |
| No `execution_timeout` (or equivalent) on tasks that call external systems | A hung connection blocks a slot indefinitely and may stall every other pipeline on a single-machine executor. | Important | A2 |

## Time and determinism

| Pattern | Why it is dangerous | Default severity | Pillar |
|---|---|---|---|
| `datetime.now()`, `date.today()`, `CURRENT_DATE` deciding which data to process | Re-running yesterday's failed run processes today's data. Backfills are impossible. | Critical | A1 |
| Dynamic `start_date` (for example `now() - timedelta(days=1)`) | The schedule shifts on every parse; runs can be skipped or duplicated. | Important | A1 |
| Code at module top level that queries a database or API | Runs on every DAG parse, loads the scheduler and can leak connections. | Important | A1 / A6 |
| Reading "the latest" record of a source without a bound | Output depends on when the task ran, not on the run's data interval. | Important | A1 |

## Loads and transactions

| Pattern | Why it is dangerous | Default severity | Pillar |
|---|---|---|---|
| `TRUNCATE` or `DELETE` then `INSERT` in separate transactions | A failure between them leaves the table empty or partial for consumers. | Critical | A3 |
| `DROP TABLE` / recreate on each run while BI tools query the table | Consumers see errors or empty results during the load; grants and indexes may be lost. | Important | A3 |
| Upsert without a declared, enforced unique key | `ON CONFLICT` has nothing to conflict on; duplicates accumulate silently. | Critical | A3 |
| Full refresh of a fact table that keeps growing, with no recorded reason | Load time and memory grow without bound; a decision nobody remembers making. | Important | A3 |
| Incremental load based on a watermark stored only in memory or a local file | A restart or a lost file causes gaps or reprocessing. | Important | A3 |

## Secrets and access

| Pattern | Why it is dangerous | Default severity | Pillar |
|---|---|---|---|
| Passwords, tokens or connection strings in code, notebooks, docs or committed config | Anyone with repository access has production access; history keeps them after deletion. | Critical | A7 |
| The same credential shared by several systems or people | One leak compromises everything; rotation becomes impossible to coordinate. | Critical | A7 |
| Pipelines connecting to sources with admin or owner accounts | A bug or injection can destroy source data. | Important | A7 |
| SQL built with f-strings or string concatenation from variable input | SQL injection; also breaks on quotes in real data. | Important | A7 |
| Secrets printed in logs (connection URIs, request headers) | Logs are usually less protected than secret stores. | Critical | A7 |

## Orchestrator misuse

| Pattern | Why it is dangerous | Default severity | Pillar |
|---|---|---|---|
| Heavy transformations of large data inside the orchestrator process when the database could do it | Memory pressure on the machine that also runs the scheduler; slow and fragile. | Important | B2 |
| One task doing extract, transform, load and notify | A failure at the end forces repeating everything; impossible to retry only the broken step. | Important | A2 |
| Passing large datasets through the orchestrator's metadata channel (for example XCom) | Bloats the metadata database; it is meant for small references. | Important | A6 |
| Very large DAGs mixing unrelated business processes | One failure blocks unrelated outputs; hard to reason about. | Improvement | B3 |

## Data quality and testing

| Pattern | Why it is dangerous | Default severity | Pillar |
|---|---|---|---|
| Tests that only check that code runs, not that results are correct | They pass while business rules are wrong. | Important | A5 |
| No row count or null check after extraction | An empty or truncated source produces a "successful" empty report. | Important | A5 |
| Business rules (payment status, stock thresholds) with no unit test | The most valuable logic is the least protected. | Important | A5 |

## Integration

| Pattern | Why it is dangerous | Default severity | Pillar |
|---|---|---|---|
| `SELECT *` from a source system | A column added or renamed upstream silently changes or breaks output. | Important | A9 |
| No check that expected columns and types exist after extraction | Schema changes surface as wrong numbers, not errors. | Important | A9 |
| Delivery to files, email or shared folders with no confirmation of success | The pipeline is green but the user never received the report. | Important | A9 |

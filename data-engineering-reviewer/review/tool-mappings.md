# Tool mappings

The pillars describe concepts. This file translates each concept to the vocabulary of the tool in use, so reviews name the right feature. Read the user's `platform.md` to know which tools apply, then use only the relevant column.

Feature names change between versions. Treat this table as a starting point and confirm against the official documentation of the installed version before recommending a specific setting.

## Contents
- Orchestrators: concept map
- Any other orchestrator
- dbt: concept map
- dbt anti-patterns
- Orchestrator + dbt together

## Orchestrators: concept map

| Concept (pillar) | Apache Airflow | Prefect | Dagster | Cron + scripts |
|---|---|---|---|---|
| Unit of work | Task in a DAG | Task in a flow | Op / asset | One script or function |
| Run's time window (A1) | Logical date / data interval from the run context | Flow parameters (pass the date explicitly) | Partition key / time window partitions | Date argument passed on the command line |
| Retries (A2) | `retries`, `retry_delay` | `retries`, `retry_delay_seconds` on task/flow | Retry policy on op/asset | Wrapper script or none |
| Timeout (A2) | `execution_timeout` | `timeout_seconds` | Run/op timeout (tags or config) | `timeout` command |
| Failure alert (A6) | `on_failure_callback`, notifiers | State hooks (`on_failure`), automations | Run failure sensors, alerts | Exit code + wrapper that notifies |
| Missed-run detection (A6) | Deadline / SLA features, external check | Automations on missing runs | Freshness policies / sensors | External check required |
| Secrets (A7) | Connections, secrets backend | Secret blocks, variables | Resources with `EnvVar`, secrets backends | Environment variables outside the repo |
| Passing small data between steps | XCom (references only) | Task return values / persisted results | IO managers | Files or tables |
| Backfill (A1) | Backfill command / catchup | Re-run flow with past parameters | Partition backfills | Loop over dates |
| Event-driven runs | Assets / datasets, deferrable sensors | Event triggers, deployments | Sensors, asset materialization | Not available |
| Concurrency limits | Pools, `max_active_runs` | Concurrency limits, work pools | Run queue / concurrency config | OS-level only |
| Data quality gate (A5) | Validation task, check operators | Validation task | Asset checks | Validation step |
| DAG/flow integrity test (A5) | Import all DAGs (DagBag) in CI | Import flows in CI | Load definitions in CI | Run scripts with `--dry-run` |

## Any other orchestrator

For a tool not listed above, map it before reviewing:

1. Read `platform.md` for the tool and version.
2. For each row of the concept map, find the tool's equivalent in its official documentation.
3. If a concept has no equivalent (for example, no failure alerts), that is itself a finding: say which pillar is uncovered and how to compensate (a wrapper, an external check).
4. Propose adding the mapping to the user's `platform.md`, so it is not rebuilt every session.

The principles never change with the tool. Only the names of the features do.

## dbt: concept map

dbt handles transformation inside the database; an orchestrator runs it. Apply pillars A3, A4, A5 and A9 to dbt projects as follows.

| Concept (pillar) | dbt feature |
|---|---|
| Load strategy per table (A3) | Materialization per model: `view`, `table`, `incremental`, `ephemeral`; `snapshot` for history |
| Idempotent incremental (A1, A3) | `incremental` with `unique_key` and an explicit `incremental_strategy` (e.g. `merge`, `delete+insert`) |
| Late data window (A3) | Incremental filter with a lookback, not only `> max(updated_at)` |
| Slowly changing dimension type 2 (A4) | Snapshots |
| Layering (A4) | Staging (one model per source table, renames and types) → intermediate → marts (facts, dimensions) |
| Data tests (A5) | Generic tests: `unique`, `not_null`, `accepted_values`, `relationships`; singular tests in SQL |
| Logic tests (A5) | Unit tests on models with fixed inputs and expected outputs |
| Blocking vs. warning (A5) | Test `severity` (`error` / `warn`) |
| Source contracts (A9) | `sources` with declared columns; source freshness checks |
| Model contracts (A9) | Enforced model contracts (columns and types) on public marts |
| Documentation and lineage (A8) | Model descriptions, generated docs |
| Secrets (A7) | `profiles.yml` reading environment variables; never committed with credentials |

## dbt anti-patterns

| Pattern | Why it is dangerous | Default severity | Pillar |
|---|---|---|---|
| `incremental` model without `unique_key` | Re-runs append duplicates | Critical | A1 / A3 |
| Incremental filter with no lookback for late or updated rows | Corrections in the source never reach the mart | Important | A3 |
| `profiles.yml` with plain credentials committed | Secret exposure | Critical | A7 |
| Marts with no tests on keys | Duplicates and broken joins go unnoticed | Important | A5 |
| `select *` in staging models | Upstream schema changes leak silently | Important | A9 |
| Business logic repeated in several marts | Reports disagree | Improvement | A4 |
| Models referencing raw tables directly instead of `source()` / `ref()` | Lineage breaks; dependency order is not guaranteed | Important | A8 |
| Tests run but results ignored (orchestrator task succeeds on test failure) | Same as a swallowed exception | Critical | A2 |

## Orchestrator + dbt together

- The orchestrator owns scheduling, extraction, retries, alerts and delivery; dbt owns in-database transformation and its tests.
- Run `dbt build` (or run + test) so tests gate downstream models, and make the orchestrator task fail when dbt exits with an error.
- Pass the run's time window to dbt as a variable when models depend on it, instead of using the current date inside SQL.
- Keep extraction and delivery out of dbt.
- Adopting dbt, or changing orchestrator, is a significant decision: use the decision method and record an ADR.

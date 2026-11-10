# A1 Idempotency and determinism

## Principle

Running the same task for the same run any number of times leaves the target in the same final state as running it once. The output depends only on the run's inputs (its data interval and the source data for that interval), never on when the task happened to execute.

This is the foundation for safe retries, backfills and recovery. Without it, every failure needs manual cleanup.

## What to check

1. **Time source.** The data selected comes from the run's logical date or data interval provided by the orchestrator, not from `now()`, `today()` or `CURRENT_DATE`.
2. **Overwrite, not append.** The task replaces the slice it owns (a date partition, a key range, a whole small table) instead of appending to it.
3. **Bounded reads.** Source queries have both a lower and an upper bound tied to the interval.
4. **No hidden state.** Watermarks, counters or "last processed" markers live in a durable place and are updated in the same transaction as the data, or are derived from the target itself.
5. **Side effects.** Emails, file drops and API calls are either safe to repeat or guarded (for example by checking whether the delivery for this run already happened).
6. **Static schedule definition.** Start dates and schedules are fixed values.

## Red flags

- Append-only inserts with retries enabled.
- A DAG whose output changes if it is triggered manually at a different hour.
- Backfilling a past date requires editing code.
- Duplicate rows that are "cleaned up" by a later step.

## Fixes

- Delete the target slice for the interval and insert it again inside one transaction (delete-insert by partition).
- Or write to a staging table and swap or merge atomically.
- For small, fully rebuilt tables, a full refresh inside one transaction is idempotent by construction.
- Use the orchestrator's templated or context-provided interval values; pass them as parameters to SQL.
- For notifications, record a delivery marker per run and check it before sending.

## Maturity signals

- Junior: knows the word; some tasks append on retry.
- Mid: every load task can be re-run safely; backfills work for any past date.
- Senior: designs storage around immutable partitions and recomputation; can rebuild any table from retained raw data.

## Sources

- Maxime Beauchemin, "Functional Data Engineering: a modern paradigm for batch data processing" (2018): pure tasks, overwriting partitions, reproducibility.
- de Ruiter et al., *Data Pipelines with Apache Airflow*, 2nd ed. (Manning, 2026): chapters on time-based scheduling and best practices.
- Reis & Housley, *Fundamentals of Data Engineering* (O'Reilly, 2022): orchestration and DataOps undercurrents.

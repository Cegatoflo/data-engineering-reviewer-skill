# A2 Failure handling

## Principle

A failure must look like a failure. Every error that leaves the output incomplete or wrong must end in a failed task, so the orchestrator stops dependent work, retries where safe, and alerts. A pipeline that reports success on failure is worse than one that crashes, because it delivers wrong data with confidence.

## What to check

1. **Propagation.** Exceptions are re-raised after logging. Helper functions do not convert errors into return values that callers ignore.
2. **Atomic tasks.** Each task does one logical unit of work (extract, transform, load, deliver) so a retry repeats only what failed.
3. **Retries.** Set for transient failures (network, locks, timeouts) with a delay; only on tasks that are idempotent (see A1).
4. **Timeouts.** Every task that touches an external system has an execution timeout, so a hung connection cannot block a worker slot indefinitely.
5. **Expected vs. unexpected errors.** "No new data today" is handled deliberately (skip or succeed with a clear log), and is distinguishable from "source unavailable".
6. **Partial writes.** A failure mid-load does not leave the target half-written (see A3).
7. **Cleanup.** Temporary files and connections are released in `finally` blocks or context managers.

## Red flags

- `try/except` around the whole task body with a log and no raise.
- Functions returning `True/False` for success that callers do not check.
- Retries configured on a task that appends.
- A failure noticed only when someone opens a report.

## Fixes

- Remove broad catches, or re-raise after logging with context (`raise ... from e`).
- Catch only specific, expected exceptions, and decide explicitly what each one means.
- Split large tasks into extract, transform, load and deliver steps.
- Add retries with delay on I/O tasks; add execution timeouts based on measured normal duration with margin.
- Add a failure callback that notifies the user (see A6).

## Maturity signals

- Junior: errors are logged; some failures are silent.
- Mid: every failure stops the run and alerts; tasks are atomic; retries are safe.
- Senior: failure modes are designed up front (what if the source is late, partial, duplicated), with documented recovery for each.

## Sources

- de Ruiter et al., *Data Pipelines with Apache Airflow*, 2nd ed. (Manning, 2026): best practices and operating in production.
- Astronomer, "DAG writing best practices in Apache Airflow" (official guide for Airflow 3): atomic tasks, retries.
- Reis & Housley, *Fundamentals of Data Engineering* (O'Reilly, 2022): chapter 3, "plan for failure".

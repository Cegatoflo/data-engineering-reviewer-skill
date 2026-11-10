# A6 Observability and alerting

## Principle

The user must learn that something is wrong before a business user does. Logs are for diagnosing; alerts are for knowing; metrics are for seeing trends before they become incidents. An alert must be actionable: if it fires, there is something to do.

## What to check

1. **Failure alerts.** A failed run notifies the user through a channel they actually watch, with the pipeline name, task, run date and a link or log location.
2. **Missing-run detection.** Something detects that an expected run did not happen or did not finish on time (a scheduler that stopped is silent by nature).
3. **Per-run metrics.** Duration, rows read, rows written and status are recorded per run in a queryable place, not only in log text.
4. **Useful logs.** Logs state what was processed (interval, counts, target) and never contain secrets or personal data.
5. **Alert hygiene.** No alerts that fire constantly and are ignored; warnings and failures are distinguished.
6. **Platform health.** Disk, memory and the scheduler process itself are monitored, since on a single machine they fail everything at once.

## Red flags

- The user finds out about failures from business users.
- Alerts go to an inbox nobody checks.
- No idea how long a pipeline normally takes, so a slowdown is invisible.
- A full disk discovered when every pipeline fails.

## Fixes

- Add an on-failure callback that sends a message to a monitored channel.
- Write one row per run to a small `pipeline_runs` table: pipeline, run date, start, end, rows in, rows out, status.
- Add a daily check that every expected pipeline has a successful run for the period.
- Monitor disk and memory on the host with a simple threshold alert.

## Maturity signals

- Junior: logs only; failures discovered manually.
- Mid: actionable failure alerts; per-run metrics; missed-run detection.
- Senior: service level objectives per critical dataset (freshness, completeness) with alerts on them; dashboards of trends.

## Sources

- Beyer, Jones, Petoff & Murphy (eds.), *Site Reliability Engineering* (O'Reilly, 2016): chapter "Monitoring Distributed Systems" (symptoms over causes, actionable alerts).
- Moses, Gavish & Vorwerck, *Data Quality Fundamentals* (O'Reilly, 2022): data observability pillars (freshness, volume, schema, distribution, lineage).
- de Ruiter et al., *Data Pipelines with Apache Airflow*, 2nd ed. (Manning, 2026): operating Airflow in production, metrics.

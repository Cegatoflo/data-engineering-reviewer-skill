# A8 Operations and continuity

## Principle

The platform must survive its builder being unavailable, and its builder must survive the platform failing. A backup that has never been restored is a hypothesis. A procedure that lives only in one person's head is a risk to the business.

## What to check

1. **Backups with restore tests.** For each stateful component (orchestrator metadata database, datamart, configuration, code), there is a backup, a retention period, and a dated record of a successful restore.
2. **Recovery objectives.** It is stated how much data can be lost (recovery point) and how long recovery may take (recovery time) for critical outputs, even informally.
3. **Rebuildability.** The server and services can be rebuilt from versioned scripts or documented steps, not from memory.
4. **Deployment.** Changes reach production through a repeatable process with a rollback path, not by editing files on the server.
5. **Runbooks.** For each critical pipeline: what it does, who consumes it, how to re-run it, common failures and their fixes.
6. **Bus factor.** Someone else (or the user after months away) could operate the platform with the documentation.
7. **Change records.** Significant changes to the platform are recorded as ADRs.

## Red flags

- "We have backups" with no date of the last restore test.
- Hotfixes applied directly on the server.
- The only documentation of the server setup is shell history.
- No one else knows that a critical report depends on a pipeline.

## Fixes

- Schedule a quarterly restore drill to a separate environment; record the result.
- Extend the existing deployment pipeline to every pipeline, one at a time.
- Write a one-page runbook per critical pipeline using the same template.
- Keep the server setup in versioned scripts; test them on a fresh VM once.

## Maturity signals

- Junior: backups assumed; manual deployment; knowledge in one head.
- Mid: tested restores; CI/CD with rollback for all pipelines; runbooks.
- Senior: infrastructure as code; documented recovery objectives met in drills; on-call ready documentation.

## Sources

- Beyer, Jones, Petoff & Murphy (eds.), *Site Reliability Engineering* (O'Reilly, 2016): chapters on postmortem culture and data integrity.
- Reis & Housley, *Fundamentals of Data Engineering* (O'Reilly, 2022): DataOps undercurrent (automation, observability, incident response).
- de Ruiter et al., *Data Pipelines with Apache Airflow*, 2nd ed. (Manning, 2026): operating Airflow in production; deployment options.

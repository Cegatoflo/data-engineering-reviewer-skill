# Maturity matrix

Use this to place the user per pillar and to raise the bar as they progress. A level is reached when the behavior is **consistent across the platform and backed by evidence** (code, tests, records), not when it was done once.

Levels describe engineering behavior, not job titles or years. Autonomy and breadth of responsibility are separate from depth: someone can own a whole platform and still be at Junior depth in testing.

## Contents
- Review pillars (A1–A9)
- Architecture (B)
- Standards (C)
- How to use the matrix

## Review pillars

| Pillar | Junior | Mid | Senior |
|---|---|---|---|
| A1 Idempotency | Knows the concept; some loads append on retry | Every load safe to re-run; backfills work for any date | Immutable raw data; any table rebuildable from retained sources |
| A2 Failure handling | Errors logged; some silent failures | Every failure stops the run and alerts; atomic tasks; safe retries and timeouts | Failure modes designed up front with documented recovery per mode |
| A3 Load strategies | One strategy everywhere or undocumented mix | Strategy per table with reasons; atomic loads; source deletes handled | Strategies tied to SLAs and growth thresholds; automated reconciliation |
| A4 Modeling | Tables mirror source queries | Star schemas, declared grain, conformed dimensions, deliberate SCD types | Bus matrix across processes; model evolves without breaking consumers |
| A5 Quality & testing | Manual checks; superficial tests | Business rules tested; blocking data checks; tests in CI | Data contracts; anomaly detection; quality metrics over time |
| A6 Observability | Logs only | Actionable alerts; per-run metrics; missed-run detection | Freshness and completeness objectives with alerting; trend dashboards |
| A7 Security | Secrets in code; shared credentials | Externalized and rotated secrets; least privilege; protected interfaces; scanning | Secrets manager with rotation; audited access; threat model |
| A8 Operations | Backups assumed; manual deploys; knowledge in one head | Tested restores; CI/CD with rollback everywhere; runbooks | Infrastructure as code; recovery objectives met in drills |
| A9 Integration | Implicit assumptions; failures surface in reports | Explicit columns; schema checks; delivery confirmation; documented assumptions | Agreed contracts with source owners; change notification |

## Architecture

| Junior | Mid | Senior |
|---|---|---|
| Chooses familiar tools; decisions not recorded | Decides with measured load and explicit trade-offs; records ADRs | Designs for evolution with fitness functions; anticipates scaling triggers; explains decisions to non-technical stakeholders and gets them adopted |

## Standards

| Junior | Mid | Senior |
|---|---|---|
| Each pipeline built differently | Shared template and conventions applied to new work | Standards adopted across the platform, enforced by CI, migrated legacy |

## How to use the matrix

- Record the current level per pillar in the user's `current-level.md`, with the evidence that supports it.
- When reviewing, compare against the next level, not the top one. The goal is the next step.
- Propose a level change only with evidence (a link to code, tests or records).
- Never present a level as a job title or as an assessment for an employer; it is a self-improvement tool.

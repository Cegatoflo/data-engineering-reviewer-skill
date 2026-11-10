# data-engineering-reviewer

An Agent Skill that helps data engineers review their own pipelines and architecture decisions with senior-level criteria.

A pipeline that runs successfully is not necessarily well designed. Idempotency, atomic loads, failure handling, data quality, security and the reasoning behind architecture decisions rarely show up in a successful run. This skill gives an AI assistant a structured set of review criteria, grounded in established data engineering literature and official tool documentation, so it can ask the questions that are easy to overlook.

The skill does not decide whether code is right or wrong. It supports the engineer's own review: it raises findings, classifies them by severity, explains the underlying principle and points to the source.

## What it does

The skill works in five modes, selected from the request:

| Mode | Use it when you... | Output |
|---|---|---|
| Code review | share a DAG, flow, dbt model, SQL or configuration | Verdict, findings by severity with fixes and sources, items to verify |
| Design review | describe something to build or compare tools | Guided questions, option evaluation, scaling triggers, draft ADR |
| Postmortem | report a failure or incident | Blameless postmortem with detection and prevention actions |
| Improvement plan | ask what to improve next or report a closed gap | Prioritized next steps with verifiable definitions of done |
| Interview practice | want to rehearse defending technical decisions | Questions, feedback and evidence-based answers |

## Review framework

### Block A: Review pillars

| Pillar | Focus |
|---|---|
| A1 Idempotency and determinism | Re-runs and backfills produce the same result; logic driven by the run's time window, not the wall clock |
| A2 Failure handling | Errors fail the task; atomic tasks; safe retries and timeouts |
| A3 Load strategies | Full refresh, delete-insert, upsert, append, snapshot: chosen per table, atomic, recorded |
| A4 Dimensional modeling | Declared grain, conformed dimensions, slowly changing dimensions |
| A5 Data quality and testing | Business rule tests that fail on wrong results; data checks at entry and before delivery |
| A6 Observability and alerting | Actionable alerts, per-run metrics, missed-run detection |
| A7 Security | Secrets outside code and logs, least privilege, protected interfaces |
| A8 Operations and continuity | Tested restores, repeatable deployment, runbooks |
| A9 Integration and data contracts | Explicit columns, schema checks, delivery confirmation |

Each pillar file states the principle, what to check, red flags, fixes, maturity signals and sources.

### Block B: Architecture decisions

A six-step method for design decisions: frame the problem, measure the load, list options (always including the simplest one and doing nothing), evaluate against fixed criteria, define measurable scaling triggers, and record an Architecture Decision Record. Technology changes are recommended only when a measurement or requirement justifies them.

### Block C: Standards and templates

Principles every pipeline template should follow, plus templates for ADRs and postmortems.

### Cross-cutting

- Anti-patterns checked before the pillar review
- Severity levels: critical, important, improvement
- A maturity matrix (junior, mid, senior) per pillar, used to raise the bar as the engineer progresses
- Tool mappings for Apache Airflow, Prefect, Dagster, cron-based scripts and dbt, plus a procedure for any other orchestrator

## Repository structure

```
data-engineering-reviewer/          The skill
  SKILL.md                          Entry point: modes, rules, navigation
  review/
    anti-patterns.md
    checklist.md
    severity.md
    tool-mappings.md
    pillars/                        A1 to A9
  architecture/
    decision-method.md
    evaluation-criteria.md
    question-bank.md
    scaling-patterns.md
    principles.md
  templates/
    template-principles.md
    adr-template.md
    postmortem-template.md
  levels/maturity-matrix.md
  modes/interview.md
  sources.md

state-templates/docs/engineering/   Optional files to copy into your own repository
```

## Design principles

**Tool-agnostic criteria.** The pillars describe concepts. A separate mapping translates them to the features of each orchestrator and of dbt, so the criteria stay valid when the stack changes.

**Separation of criteria and state.** The skill contains stable review criteria. Information that changes over time (your platform, known gaps, current maturity level, decisions and lessons) lives in your own repository, in `docs/engineering/`. The skill reads those files when they exist and proposes updates at the end of each session.

**Progressive disclosure.** `SKILL.md` is short and points the assistant to the files each mode needs, following Anthropic's skill authoring guidelines.

**Explicit review stance.** The skill opens with the verdict, puts critical findings first, avoids inventing findings, distinguishes what the code shows from what is assumed, and always lists what cannot be verified from code alone.

## Installation

**Claude (web and desktop apps).** Compress the `data-engineering-reviewer/` folder into a zip file with the folder at its root, then upload it under Settings > Capabilities > Skills. A packaged `.skill` file is also available in the Releases section of this repository.

**Claude Code.** Copy the folder to `~/.claude/skills/data-engineering-reviewer/` for personal use, or to `.claude/skills/data-engineering-reviewer/` inside a project.

**Other AI assistants.** Use the contents of `SKILL.md` as system or project instructions and provide the reference files the selected mode needs. All files are plain Markdown with relative links.

## Optional: project state files

To let the skill track progress over time, copy `state-templates/docs/engineering/` into the root of your repository and fill it in:

| File | Purpose |
|---|---|
| `platform.md` | Current stack, versions and constraints |
| `gaps-tracker.md` | Known gaps and their status |
| `current-level.md` | Maturity level per pillar, with evidence |
| `scaling-thresholds.md` | Measured triggers for scaling decisions |
| `load-policy.md` | Load strategy per target table |
| `learnings.md` | Lessons learned and advice already considered |
| `adr/` | Architecture decision records |
| `postmortems/` | Incident reviews |

Do not store credentials, hostnames, IP addresses or customer data in these files. Keep them in a private repository if they describe internal systems.

## Example requests

- "Review this Airflow DAG before it goes to production."
- "I want to move the datamart to incremental loads. How should I approach it?"
- "Yesterday's stock report was empty and nobody noticed until 10 am."
- "Which gaps should I work on this month?"
- "Should I add dbt to the current stack?"

## Sources

The criteria are based on the following references. Full details, including relevant chapters, are in `data-engineering-reviewer/sources.md`.

- Joe Reis and Matt Housley, *Fundamentals of Data Engineering* (O'Reilly, 2022)
- Martin Kleppmann and Chris Riccomini, *Designing Data-Intensive Applications*, 2nd edition (O'Reilly, 2026)
- Julian de Ruiter et al., *Data Pipelines with Apache Airflow*, 2nd edition (Manning, 2026)
- Ralph Kimball and Margy Ross, *The Data Warehouse Toolkit*, 3rd edition (Wiley, 2013)
- Barr Moses, Lior Gavish and Molly Vorwerck, *Data Quality Fundamentals* (O'Reilly, 2022)
- Betsy Beyer et al. (eds.), *Site Reliability Engineering* (O'Reilly, 2016)
- Neal Ford, Rebecca Parsons, Patrick Kua and Pramod Sadalage, *Building Evolutionary Architectures*, 2nd edition (O'Reilly)
- Maxime Beauchemin, "Functional Data Engineering: a modern paradigm for batch data processing" (2018)
- Michael Nygard, "Documenting Architecture Decisions" (2011)
- OWASP, Secrets Management Cheat Sheet
- Official documentation of Apache Airflow, Prefect, Dagster and dbt
- Anthropic, Agent Skills authoring best practices

## Limitations

- A review is limited to what the assistant is shown. It cannot observe production behavior, so every review includes items to verify in the real environment.
- It is not a security audit. Serious security findings should be confirmed by an independent review.
- Tool features change between versions. Specific settings should be confirmed against the official documentation of the installed version.
- The skill is not affiliated with or endorsed by the authors of the referenced works.

## Contributing

Issues and pull requests are welcome, especially for new tool mappings, additional anti-patterns and corrections to the criteria. Please cite a verifiable source for any new principle.

## License

Released under the MIT License. See [LICENSE](LICENSE).

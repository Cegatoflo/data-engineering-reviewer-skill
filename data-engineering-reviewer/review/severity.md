# Severity levels

Classify every finding with one level. Severity reflects the consequence in the user's context, not how far the code is from a textbook.

## 🔴 Critical

The pipeline can produce incorrect data without anyone noticing, lose data, or expose systems or credentials.

Typical examples:
- Errors swallowed so the run reports success on failure.
- Retries or re-runs that duplicate or delete data.
- Credentials in code or logs; shared or over-privileged accounts.
- Non-atomic destructive loads on tables people read.
- A business rule that is demonstrably wrong.

Expectation: fix before the next production run, or document an explicit, temporary acceptance of the risk.

## 🟠 Important

The pipeline works today but recovery, detection or maintenance will fail when something goes wrong or grows.

Typical examples:
- No alerting on failure; the user learns from business users.
- Impossible to reprocess a past date.
- No tests on business rules.
- Backups that have never been restored.
- Unbounded full refresh of a growing table.

Expectation: plan it into the next improvement cycle.

## 🔵 Improvement

Quality, clarity or efficiency gains with no current risk.

Typical examples:
- Naming, structure, duplication across DAGs.
- Missing documentation of a reasonable decision.
- Small performance gains with no current pain.

Expectation: fix opportunistically, or when the code is touched again.

## Adjusting severity

Raise one level when:
- The output drives money, payments, invoices or stock decisions.
- The failure would be silent.
- Several pipelines share the problem.

Lower one level when:
- The table is small, rebuilt quickly and no one reads it during the load.
- There is an effective compensating control (a validation downstream that would catch it, a manual check that actually happens).

State the reason whenever severity is adjusted.

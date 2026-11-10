# Review checklist

Use this to decide which pillars apply, then read only those pillar files (links are in SKILL.md). Each line is a question to answer from the code; "unknown" is a valid answer and goes to the "Verificar" section of the review.

## Which pillars apply

| The code contains... | Pillars |
|---|---|
| A DAG or scheduled job | A1, A2, A6 |
| Extraction from a source system | A2, A7, A9 |
| Writes to a database table | A1, A3 |
| Fact or dimension tables, a datamart | A3, A4 |
| Business rules or calculations | A5 |
| Credentials, connections, network calls | A7 |
| Delivery (files, email, shared folders, APIs) | A2, A6, A9 |
| Deployment, server, service or backup config | A7, A8 |

## Quick questions per pillar

**A1 Idempotency**
- Does re-running the same run produce the same final state?
- Does the code select data by the run's data interval, not the wall clock?

**A2 Failure handling**
- Does every error path end in a failed task?
- Is each task a single unit that either completes or fails cleanly?
- Are retries and timeouts set and safe?

**A3 Load strategies**
- Is the load strategy of each target table explicit and recorded?
- Are destructive steps and inserts in one transaction or swapped atomically?
- Is the uniqueness assumption of an upsert enforced by a constraint?

**A4 Dimensional modeling**
- Is the grain of each fact table stated?
- How are changes in dimension attributes handled?

**A5 Data quality and testing**
- Are business rules covered by tests that fail on wrong results?
- Are there checks on volume, nulls, duplicates and ranges after extraction and before delivery?

**A6 Observability**
- Will the user know about a failure before a business user does?
- Is duration and row count recorded per run?

**A7 Security**
- Are secrets outside the code and logs?
- Does each connection use the minimum privilege it needs?

**A8 Operations**
- Can this be restored, re-deployed and run by someone following a document?

**A9 Integration**
- What happens if the source adds, renames or removes a column?
- Is successful delivery confirmed?

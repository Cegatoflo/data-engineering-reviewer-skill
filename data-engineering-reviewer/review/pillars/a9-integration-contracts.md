# A9 Integration and data contracts

## Principle

Pipelines depend on systems the data engineer does not control: ERPs, office platforms, file shares, other teams' databases. Every boundary is a contract, explicit or not. Make the expectations explicit and check them at the boundary, so upstream changes fail loudly at the entrance instead of quietly corrupting the output.

## What to check

1. **Explicit columns.** Extraction selects named columns, never `SELECT *`.
2. **Schema check at entry.** After extraction, expected columns and types are verified; unexpected changes fail the task or raise a visible warning.
3. **Semantic assumptions.** Assumptions about source meaning (status codes, document types, cancellation flags, currency, units) are written down next to the query.
4. **Source load.** Queries on operational systems (for example an ERP database) are bounded, use indexed filters, and run at times that do not hurt business users.
5. **Delivery confirmation.** Uploads to shared folders, cloud storage or email confirm success (status code, file exists, size > 0) and fail otherwise.
6. **External API behavior.** Rate limits, pagination, token expiry and partial responses are handled.
7. **Version changes.** Upgrades of the source system are anticipated: someone checks the pipelines when the ERP or platform version changes.
8. **Ownership.** For each external dependency it is known who owns it and whom to contact.

## Red flags

- A source column renamed after an ERP update, and reports showing zeros for a week.
- File uploads that "succeed" while the target folder is unchanged.
- Heavy extraction queries running during business hours on the ERP.
- Status code meanings known only from memory.

## Fixes

- Replace `SELECT *` with explicit column lists.
- Add a schema assertion step after each extraction.
- Document a small contract per source: tables, columns, meaning, owner, refresh expectations.
- Verify every delivery and fail the task if verification fails.
- Add an item to the upgrade checklist of each source system: "re-run pipeline validations".

## Maturity signals

- Junior: implicit assumptions; failures surface in reports.
- Mid: explicit columns, schema checks and delivery confirmation; documented assumptions.
- Senior: formal data contracts agreed with source owners; change notification process.

## Sources

- Reis & Housley, *Fundamentals of Data Engineering* (O'Reilly, 2022): chapter on source systems and ingestion.
- Kleppmann & Riccomini, *Designing Data-Intensive Applications*, 2nd ed. (O'Reilly, 2026): encoding and schema evolution.
- Moses, Gavish & Vorwerck, *Data Quality Fundamentals* (O'Reilly, 2022): schema as a data observability pillar.

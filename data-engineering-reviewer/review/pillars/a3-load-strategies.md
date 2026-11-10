# A3 Load strategies

## Principle

Every target table has one deliberate load strategy, chosen for its size, change pattern and consumers, and recorded. Mixed strategies are fine; unexplained ones are not. No strategy is wrong in itself: a full refresh with `TRUNCATE` can be idempotent and correct; an upsert can be dangerous without an enforced key. Judge the strategy against the table, its transactions, its dependencies and its recovery path.

## Strategies and when they fit

| Strategy | Fits when | Watch for |
|---|---|---|
| Full refresh | Small to medium tables, cheap to rebuild, source has no reliable change marker | Must be atomic for readers; cost grows with the table |
| Delete-insert by partition | Facts organized by date; late corrections within a known window | Partition key must match the run's interval |
| Upsert (merge, `ON CONFLICT`) | Dimensions and mutable records with a stable business key | Needs a unique constraint; does not handle deletes in the source |
| Append-only | Immutable events, logs | Needs deduplication or a run key to be retry-safe |
| Snapshot | Need history of state over time (stock levels, balances) | Storage growth; define retention |

## What to check

1. **Recorded choice.** The strategy for this table appears in the user's `load-policy.md` or the code explains it.
2. **Atomicity for readers.** Destructive steps and inserts happen in one transaction, or the new data is built in a staging table and swapped in.
3. **Enforced keys.** Upserts rely on a declared unique or primary key, not on an assumption.
4. **Source deletes.** For incremental and upsert loads, there is a stated answer to "what happens when a row is deleted or cancelled in the source".
5. **Referential effects.** Truncating or deleting a parent table does not break or cascade into children unexpectedly.
6. **Late data.** Incremental windows cover how late corrections can arrive in the source.
7. **Growth.** A full refresh of a growing table has a threshold at which it will change strategy.

## Red flags

- `TRUNCATE` committed before the `INSERT` finishes.
- Upserts on tables with no unique constraint.
- Incremental loads that never see updates or cancellations.
- Nobody can say why a table is fully refreshed.

## Fixes

- Wrap delete and insert in one transaction; or load to `table_new` and rename in one transaction.
- Add the unique constraint, then clean existing duplicates first.
- Add a reconciliation step (counts or sums per period between source and target).
- Extend the incremental window by the known correction lag.
- Record the strategy per table in `load-policy.md`.

## Maturity signals

- Junior: one strategy everywhere, or a mix nobody documented.
- Mid: strategy chosen per table with reasons; loads are atomic; deletes handled.
- Senior: strategies tied to SLAs and growth thresholds; reconciliation automated.

## Sources

- Kimball & Ross, *The Data Warehouse Toolkit*, 3rd ed. (Wiley, 2013): fact table types (transaction, periodic snapshot, accumulating snapshot).
- Astronomer, "DAG writing best practices in Apache Airflow": incremental loads by last-modified date or sequence ID.
- Beauchemin, "Functional Data Engineering" (2018): overwriting partitions.

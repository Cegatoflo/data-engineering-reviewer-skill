# Load policy

One row per target table. Strategies: full refresh · delete-insert by partition · upsert · append-only · snapshot.

| Table | Pipeline | Grain | Strategy | Unique key (enforced?) | Atomic? | Source deletes handled | Late data window | Reason | Reviewed |
|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | |

## Rules

- A table without a row here has no approved strategy.
- Upserts require an enforced unique constraint on the key.
- Destructive steps and inserts run in one transaction or via staging swap.
- Changing the strategy of a large or shared table requires an ADR.

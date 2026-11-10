# Template principles

What every pipeline template and new pipeline should include from day one. The concrete template (for a specific orchestrator version) lives in the user's repository; this list is what to check it against. If the user has no template yet, help them create one in `docs/engineering/templates/`.

## Every pipeline

1. **Header documentation.** Purpose, business owner, consumers, schedule, expected duration, sources, targets, load strategy.
2. **Fixed schedule definition.** Static start date; catch-up and backfill behavior chosen deliberately.
3. **Interval-driven logic.** Every query is parameterized by the run's data interval.
4. **Atomic tasks.** Separate extract, validate, transform, load and deliver.
5. **Failure semantics.** No broad catches; retries with delay on I/O tasks; execution timeouts; failure callback that alerts.
6. **Idempotent load.** Delete-insert by interval, staging swap, or upsert on an enforced key, inside a transaction.
7. **Validation gate.** A task that checks volume, keys and required columns and fails on violation.
8. **Run metrics.** Rows in, rows out, duration and status recorded per run.
9. **Connections by reference.** Credentials retrieved from the connection store; no secrets in code.
10. **Delivery confirmation.** Every external delivery is verified.
11. **Testability.** Business logic lives in plain functions that can be unit tested without the orchestrator.

## Every server or platform change

1. Scripted or documented steps that rebuild it.
2. Secrets outside the repository.
3. Network exposure reviewed.
4. Backup covered and restore tested.
5. Monitoring of the new component.
6. ADR if the change is significant.

## Conventions

The user's naming and folder conventions belong in their repository. A good convention makes these obvious from names alone: business area, process, environment, and whether a task extracts, transforms, loads or delivers.

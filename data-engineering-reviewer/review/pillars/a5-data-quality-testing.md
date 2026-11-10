# A5 Data quality and testing

## Principle

Code tests protect logic; data checks protect against the world. A pipeline needs both: unit tests on business rules, and validations that stop or flag a run when the data itself is wrong. A test that cannot fail on a wrong result is not a test.

## What to check

1. **Business rule tests.** The rules that carry business meaning (payment status, stock thresholds, cost calculations, reconciliation) have unit tests with small, explicit inputs and expected outputs, including edge cases (zero, null, partial, negative, boundary values).
2. **DAG integrity tests.** A test imports every DAG and fails on import errors, cycles or missing required arguments. It runs in CI before deployment.
3. **Data checks after extraction.** Row count within an expected range, required columns present, no nulls in keys, no duplicates on keys.
4. **Data checks before delivery.** Totals reconcile with the source for the period; values within plausible ranges; no empty report sent.
5. **Failing behavior.** A failed check either fails the task (blocking) or raises a visible warning (non-blocking), and the choice is deliberate per check.
6. **Test data.** Tests use synthetic data, never production extracts with customer information.

## Red flags

- Tests that assert only that a function returns a DataFrame.
- No test touches the most important business calculation.
- A report sent with zero rows because the source was empty.
- Checks that log a warning nobody reads.

## Fixes

- Start with the riskiest rule: write tests from real past incidents or edge cases.
- Add a `validate_<dataset>` task between transform and load that raises on violations.
- Express expected ranges in configuration, with the reasoning next to them.
- Add a DAG import test to CI.

## Maturity signals

- Junior: manual checks; tests absent or superficial.
- Mid: business rules tested; blocking data checks on critical pipelines; CI runs tests.
- Senior: data contracts with sources; anomaly detection on volumes and distributions; quality metrics tracked over time.

## Sources

- Moses, Gavish & Vorwerck, *Data Quality Fundamentals* (O'Reilly, 2022): dimensions of data quality, data observability.
- de Ruiter et al., *Data Pipelines with Apache Airflow*, 2nd ed. (Manning, 2026): chapter on testing.
- Reis & Housley, *Fundamentals of Data Engineering* (O'Reilly, 2022): DataOps undercurrent.

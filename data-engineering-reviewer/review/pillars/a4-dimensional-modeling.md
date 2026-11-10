# A4 Dimensional modeling

## Principle

A datamart serves analysis. Facts record measurable events at a single, declared grain; dimensions describe the context of those events. The model is designed from the business questions, not from the shape of the source tables.

## What to check

1. **Declared grain.** Each fact table states what one row represents (one invoice line, one payment, one product per warehouse per day). Mixed grains in one table are a defect.
2. **Additive measures.** Measures are clearly additive, semi-additive (balances, stock: not summable over time) or non-additive (ratios, prices). Ratios are computed from stored components, not stored pre-computed and summed.
3. **Conformed dimensions.** The same dimension (customer, product, date, salesperson) is shared across facts, so reports agree.
4. **Surrogate vs. business keys.** Dimensions have a stable key that survives changes in the source system.
5. **Changing attributes.** For each dimension attribute that can change (a customer's zone, a product's category), the model states whether history is kept (slowly changing dimension type 2) or overwritten (type 1), and why.
6. **Unknown members.** Facts that arrive before their dimension, or with null references, point to an explicit "unknown" row instead of being dropped.
7. **Date dimension.** A real date dimension exists instead of date logic repeated in every report.

## Red flags

- A fact table where some rows are lines and others are headers.
- Reports that disagree because each joins a different customer table.
- History rewritten when a salesperson or zone changes, so past results move.
- Facts silently lost in inner joins to dimensions.

## Fixes

- Write one sentence per fact table: "One row represents...". Split tables that fail it.
- Pick type 1 or type 2 per attribute based on whether the business needs "as it was" or "as it is" reporting.
- Add an "unknown" member (for example key -1) to each dimension and use left joins on load.
- Build a shared date dimension once.

## Maturity signals

- Junior: tables mirror source queries; reports built on wide flat tables.
- Mid: star schemas with declared grain and conformed dimensions; deliberate type 1 / type 2 choices.
- Senior: bus matrix across business processes; model evolves without breaking consumers.

## Sources

- Kimball & Ross, *The Data Warehouse Toolkit*, 3rd ed. (Wiley, 2013): four-step dimensional design, grain, conformed dimensions, slowly changing dimensions.
- Reis & Housley, *Fundamentals of Data Engineering* (O'Reilly, 2022): data modeling in the transformation stage.

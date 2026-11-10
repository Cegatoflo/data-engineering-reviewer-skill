# Scaling patterns

Scale when a measurement says so, one step at a time, cheapest step first. Each ladder below is generic; the user's concrete thresholds belong in their `scaling-thresholds.md`.

## Contents
- Data volume and transformation
- Orchestration capacity
- Single host
- Number of pipelines
- Consumers and delivery
- People

## Data volume and transformation

1. Process in the database: push filters, joins and aggregations into SQL where the data already lives.
2. Process in chunks or by partition instead of loading everything into memory.
3. Use columnar intermediate formats (Parquet) and lighter, faster single-machine engines when Python processing is needed.
4. Move heavy transformations to a dedicated engine (a warehouse or a distributed engine such as Spark), orchestrated by the scheduler rather than run inside it.

**Typical triggers to move up a step:** peak memory of a task above a set fraction of host memory; task duration approaching its window; data per run growing faster than the host can absorb.

**Do not** jump to a distributed engine because files are "large". Large Excel deliverables are a presentation format problem, not a compute problem.

## Orchestration capacity

1. Single-machine executor with sensible concurrency limits and pools.
2. Tune concurrency, pools and priorities; spread schedules to avoid peaks.
3. Distributed executor (workers on several machines) or containerized tasks.

**Typical triggers:** tasks waiting for slots regularly; schedule delays; host saturated at peak.

## Single host

1. One VM with monitoring of CPU, memory and disk.
2. Separate the metadata database and the heaviest workloads from the scheduler host.
3. Multiple hosts, or managed services.

**Typical triggers:** sustained resource saturation; the cost of an outage of the single host exceeding the cost of redundancy.

## Number of pipelines

1. Copy-and-adapt per pipeline (acceptable for very few).
2. Shared library and a pipeline template; conventions documented.
3. Configuration-driven pipelines (one generic pipeline, many configs) where patterns repeat.

**Typical triggers:** the same fix applied in more than three places; new pipelines taking as long as the first one.

## Consumers and delivery

1. Files and emails per request.
2. A modeled datamart that BI tools read directly; files only where required.
3. Semantic layer or governed datasets shared by many reports.

**Typical triggers:** the same number calculated differently in two reports; frequent ad-hoc requests for the same data.

## People

1. One person, with documentation and runbooks.
2. A second person who can operate (not necessarily build).
3. Shared ownership with code review.

**Typical trigger:** the business cannot tolerate a week without the builder.

## Fitness functions

For each trigger, prefer an automated check: a metric recorded per run and an alert when it crosses 70–80% of the threshold, so the decision is made before the limit is hit.

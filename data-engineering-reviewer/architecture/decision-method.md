# Architecture decision method

Follow these six steps for any design or technology decision. Copy the checklist into the response:

```
Decision progress:
- [ ] 1. Frame the problem
- [ ] 2. Measure the load
- [ ] 3. List options (including the simplest)
- [ ] 4. Evaluate against fixed criteria
- [ ] 5. Define scaling triggers
- [ ] 6. Record the ADR
```

## 1. Frame the problem

State in two or three sentences:
- The business need and who consumes the result.
- What happens to the business if it fails, is late or is wrong.
- What is fixed (systems that cannot change, deadlines, budget) and what is open.

If the user cannot state the consequence of failure, help them find it before going further; it determines how much robustness is justified.

## 2. Measure the load

Describe the load with numbers, current and expected in 12 months:
- Data volume per run and in total (rows, MB/GB).
- Frequency and acceptable latency (daily by 7 am? every 15 minutes?).
- Number of pipelines, consumers and people who will maintain it.
- Current duration and resource usage of comparable pipelines.

If numbers are missing, tell the user exactly how to measure them and give a provisional recommendation that is explicitly conditional on the measurement. Do not decide on intuition about "big data".

## 3. List options

Always include, as real options:
- **Do nothing / keep current approach**, with what it costs.
- **The simplest thing that could work** with existing tools.
- One or two more ambitious options.

Prefer at most three or four options; more confuses the decision.

## 4. Evaluate

Score each option against [evaluation-criteria.md](evaluation-criteria.md). Present a compact table, then a recommendation in one paragraph that names the deciding criterion.

## 5. Define scaling triggers

Turn "will this scale?" into measurable conditions, using [scaling-patterns.md](scaling-patterns.md):
- "Keep option X until <metric> exceeds <threshold>; then move to Y."
- Propose adding the trigger to the user's `scaling-thresholds.md`.
- Where possible, suggest an automated check that warns when the trigger is approached (a fitness function).

## 6. Record

For significant decisions, draft an ADR with [../templates/adr-template.md](../templates/adr-template.md): context, options, decision, consequences, triggers to revisit. A decision is significant when it is hard to reverse, affects several pipelines, adds a technology, or changes how the platform is operated.

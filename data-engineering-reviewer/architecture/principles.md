# Architecture principles

Use these to justify recommendations. Cite the source when it helps the user study the idea further.

## From *Fundamentals of Data Engineering* (Reis & Housley, 2022), chapter 3

- **Choose common components wisely.** Prefer components that solve many needs and that the team can run well.
- **Plan for failure.** Design for what happens when each component fails; know recovery time and recovery point.
- **Architect for scalability.** Scale up and down with demand, and only as much as needed.
- **Architecture is leadership.** Decisions must be explained and adopted, not only made.
- **Always be architecting.** Architecture evolves continuously with the business.
- **Build loosely coupled systems.** Components that can change independently.
- **Make reversible decisions.** Favor choices that can be undone cheaply.
- **Prioritize security.** Security is part of the design, not an afterthought.
- **Embrace FinOps.** Cost is a design parameter.

The book's data engineering lifecycle (generation, storage, ingestion, transformation, serving) and its undercurrents (security, data management, DataOps, data architecture, orchestration, software engineering) are a useful map to check that a design covers every concern.

## From *Designing Data-Intensive Applications* (Kleppmann & Riccomini, 2nd ed., 2026)

- **Describe load before discussing scalability.** Scalability only means something relative to a specific load parameter.
- **Reliability, scalability and maintainability** are separate goals; maintainability (operability, simplicity, evolvability) dominates long-term cost.
- **Trade-offs, not answers.** The book gives vocabulary to articulate trade-offs; the decision depends on context.

## From *Building Evolutionary Architectures* (Ford, Parsons, Kua & Sadalage, 2nd ed.)

- **Fitness functions.** Encode architectural goals as automated checks (duration under a limit, memory under a threshold, no secret in code), so the architecture is governed continuously.
- **Incremental change.** Design so the system can evolve in small steps.

## From *Functional Data Engineering* (Beauchemin, 2018)

- **Reproducibility through immutability.** Immutable raw data plus versioned logic lets any output be recomputed.
- **Pure tasks** that overwrite their own partition.

## Simplicity for small teams

Not from a single source, but consistent with all of the above: each additional component must earn its place by solving a measured problem. The cost of complexity is paid by the few people who maintain it.

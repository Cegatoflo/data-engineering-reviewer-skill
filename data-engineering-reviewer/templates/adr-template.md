# ADR template

Save ADRs in the user's repository as `docs/engineering/adr/ADR-NNN-short-title.md`, numbered in order and never deleted. A superseded ADR stays, with its status changed and a link to the one that replaces it.

Draft ADRs in English for consistency with the rest of the repository documentation, unless the user asks otherwise.

```markdown
# ADR-NNN: <Decision title>

- **Status:** Proposed | Accepted | Superseded by ADR-XXX | Deprecated
- **Date:** YYYY-MM-DD
- **Deciders:** <name>

## Context
What problem or need forces this decision. Business consequence of getting it wrong.
Measured load and constraints (volumes, frequency, latency, people, budget).

## Options considered
1. <Option A — usually "keep current approach">
2. <Option B — simplest viable change>
3. <Option C>

## Evaluation
| Criterion | A | B | C |
|---|---|---|---|
| Fit to requirement | | | |
| Operable by the team | | | |
| Reversibility | | | |
| Failure modes | | | |
| Security | | | |
| Cost (incl. time) | | | |
| Learning curve | | | |

## Decision
What was chosen and the deciding criterion.

## Consequences
What becomes easier, what becomes harder, what new risks appear.

## Revisit when
Measurable triggers that would reopen this decision.
```

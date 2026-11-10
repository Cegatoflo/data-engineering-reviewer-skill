# Evaluation criteria

Evaluate every option against the same criteria so decisions are comparable over time. Score each as ✅ good fit, ⚠️ acceptable with caveats, ❌ poor fit, and add one line of reasoning.

| Criterion | Question | Why it matters for a small team |
|---|---|---|
| Fit to requirement | Does it meet the measured load and latency with margin? | Over-meeting a requirement is a cost, not a benefit. |
| Operable by the team | Can the people who maintain it (often one person) install, monitor, upgrade and debug it, at 3 am? | Every component added is one more thing a small team must know. |
| Reversibility | How hard is it to undo or replace in a year? | Reversible decisions can be made quickly; irreversible ones deserve the full method. |
| Failure modes | How does it fail, and is failure visible? | Prefer systems that fail loudly and recover simply. |
| Security | Does it add exposed surfaces, credentials or data copies? | Each new copy of data and each new credential is a liability. |
| Cost | License, hosting, and above all the user's time to build and maintain. | Time is the scarcest resource on a one-person team. |
| Learning curve | Can the user become competent before it reaches production? | Running a tool in production while learning it multiplies risk. |
| Ecosystem fit | Does it integrate with the current stack and the sources and consumers in use? | Integration effort is usually underestimated. |
| Career value | Does it build skills that transfer to the roles the user is targeting? | A tie-breaker only, never the main reason. |

## Rules

- Career value or novelty never outweighs operability or fit.
- If two options tie, prefer the more reversible and the simpler one.
- State explicitly which criterion decided the outcome.

# Postmortem template

Blameless: the question is which control was missing, not who made the mistake. Save in the user's repository as `docs/engineering/postmortems/YYYY-MM-DD-short-title.md`.

```markdown
# Postmortem: <short title>

- **Date of incident:** YYYY-MM-DD
- **Detected:** YYYY-MM-DD HH:MM — by <monitoring | business user | the engineer>
- **Resolved:** YYYY-MM-DD HH:MM
- **Severity:** Critical | Important | Minor

## Impact
Which outputs were wrong, late or missing; who was affected; for how long.

## Timeline
- HH:MM — what happened / what was done

## Root cause
The technical cause, then the deeper cause (why it was possible and why it was not caught).

## Detection
How it was detected, and how it should have been detected.
Time between start and detection.

## Resolution
What was done to recover, including data corrections.

## Actions
| Action | Type (prevent / detect / recover) | Gap or pillar | Status |
|---|---|---|---|

## Lesson
One sentence for learnings.md.
```

## Rules

- At least one action of type "detect": the next occurrence must be caught by a test, a check or an alert, not by a person.
- If the detection came from a business user, the postmortem is incomplete until an alert exists.
- Link actions to the gaps tracker.

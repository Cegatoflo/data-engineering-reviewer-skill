# A7 Security

## Principle

Secrets never live in code, documents or logs. Every identity has the minimum access its job needs. Security findings are verified in the real environment and remediated through an authorized process; a code review is not a security certification.

## What to check

1. **Secret storage.** Credentials come from the orchestrator's connection store, environment variables managed outside the repository, or a secrets manager. Nothing in code, notebooks, config committed to version control, or documentation.
2. **Repository history.** If a secret was ever committed, deleting it is not enough: it must be rotated, because history keeps it.
3. **Unique credentials.** Each system and each purpose has its own credential, so one leak does not open everything and rotation is possible.
4. **Least privilege.** Extraction users can only read what they need; load users can only write to their schemas; no pipeline uses an admin account.
5. **Exposed interfaces.** Admin UIs and APIs (orchestrator web server, APIs, file shares) require authentication, are not exposed beyond the network that needs them, and use TLS where traffic crosses networks.
6. **Query safety.** Parameters are passed with bind variables, not string formatting.
7. **Logs and outputs.** No connection strings, tokens, headers or personal data in logs, emails or error messages.
8. **Data sent outside.** Files shared to folders, email or cloud storage contain only what recipients should see.

## Red flags

- A password visible in a DAG file or a markdown runbook.
- The same password for the database, the orchestrator and the file share.
- The orchestrator UI reachable without authentication or from the whole network.
- A shared service account with admin rights used by every pipeline.

## Fixes

- Move secrets to the connection store or a secrets backend; reference them by connection ID.
- Rotate any credential that ever appeared in the repository or in documents, then clean history if needed.
- Create separate read-only and write accounts per system.
- Restrict network exposure with firewall rules; require authentication; enable TLS.
- Add a secret scanner to CI.

Never reproduce a found secret in the review output. Point to its location only.

## Maturity signals

- Junior: secrets in code or config; shared credentials.
- Mid: secrets externalized and rotated; least privilege; interfaces protected; secret scanning in CI.
- Senior: secrets manager with rotation, audit logs reviewed, threat model for the platform.

## Sources

- OWASP, "Secrets Management Cheat Sheet" (OWASP Cheat Sheet Series).
- de Ruiter et al., *Data Pipelines with Apache Airflow*, 2nd ed. (Manning, 2026): chapter "Securing Airflow".
- Reis & Housley, *Fundamentals of Data Engineering* (O'Reilly, 2022): security undercurrent and chapter 10.

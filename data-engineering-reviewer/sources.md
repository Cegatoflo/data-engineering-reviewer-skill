# Sources

Verified references the skill relies on. Cite these when a principle helps the user study further. Do not cite anything that is not on this list without verifying it first.

## Contents
- Core books
- Practice guides
- How to recommend reading

## Core books

| Reference | Edition | Use for | Key parts |
|---|---|---|---|
| Joe Reis & Matt Housley, *Fundamentals of Data Engineering: Plan and Build Robust Data Systems* | 1st ed., O'Reilly, 2022 | Overall framework; architecture principles; undercurrents | Ch. 2 lifecycle and undercurrents; ch. 3 good data architecture; ch. 5–9 source systems, storage, ingestion, transformation, serving; ch. 10 security |
| Martin Kleppmann & Chris Riccomini, *Designing Data-Intensive Applications* | 2nd ed., O'Reilly, 2026 (the 2017 1st edition is superseded) | Scalability, reliability, trade-offs, schema evolution, transactions | Early chapters on describing load and trade-offs; chapters on encoding/evolution and transactions |
| Julian de Ruiter, Ismael Cabral, Kris Geusebroek, Daniel van der Ende & Bas Harenslak, *Data Pipelines with Apache Airflow* | 2nd ed., Manning, 2026 (revised for Airflow 3) | Airflow design, testing, operations, security | Ch. 3 time-based scheduling; ch. 4 asset-aware scheduling; ch. 10 testing; ch. 12 best practices; ch. 15 operating in production; ch. 16 securing Airflow |
| Ralph Kimball & Margy Ross, *The Data Warehouse Toolkit: The Definitive Guide to Dimensional Modeling* | 3rd ed., Wiley, 2013 | Dimensional modeling, grain, SCD, fact table types | Early chapters: four-step design process, fact and dimension techniques |
| Barr Moses, Lior Gavish & Molly Vorwerck, *Data Quality Fundamentals* | O'Reilly, 2022 | Data quality, data observability | Data observability pillars; testing and monitoring data |
| Betsy Beyer, Chris Jones, Jennifer Petoff & Niall Richard Murphy (eds.), *Site Reliability Engineering* | O'Reilly, 2016 (free online at sre.google) | Monitoring, alerting, postmortems, data integrity | "Monitoring Distributed Systems"; "Postmortem Culture"; "Data Integrity" |
| Neal Ford, Rebecca Parsons, Patrick Kua & Pramod Sadalage, *Building Evolutionary Architectures* | 2nd ed., O'Reilly | Fitness functions, incremental change | Fitness functions chapters |

## Practice guides

| Reference | Use for |
|---|---|
| Maxime Beauchemin, "Functional Data Engineering: a modern paradigm for batch data processing" (Medium, January 2018) | Idempotency, pure tasks, partition overwrite, reproducibility |
| Astronomer, "DAG writing best practices in Apache Airflow" (Airflow 3 guide, astronomer.io/docs) | Idempotent DAGs, avoiding wall-clock dates, incremental loads, retries, offloading heavy processing |
| Apache Airflow official documentation, "Best Practices" (airflow.apache.org) | Version-specific guidance for the installed release |
| Prefect official documentation (docs.prefect.io) | Retries, timeouts, state hooks, secrets, deployments |
| Dagster official documentation (docs.dagster.io) | Assets, partitions, asset checks, sensors |
| dbt official documentation (docs.getdbt.com), including "Best practices" guides | Materializations, incremental models, snapshots, tests, contracts, project structure |
| OWASP Cheat Sheet Series, "Secrets Management Cheat Sheet" (cheatsheetseries.owasp.org) | Secret storage, rotation, least privilege |
| Michael Nygard, "Documenting Architecture Decisions" (Cognitect blog, 2011) | ADR format |

## How to recommend reading

- Tie each recommendation to an open gap: "For gap X, read chapter Y."
- Prefer one chapter over a whole book.
- For version-specific behavior of tools (configuration options, APIs), point to the official documentation of the version in the user's `platform.md`, because books and this skill can lag behind releases.

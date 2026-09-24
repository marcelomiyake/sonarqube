# ADR-0002: Use PostgreSQL for SonarQube State

- **Status:** Implemented (retrospective)
- **Recorded:** 2026-09-25
- **Original decision date:** Unknown from repository evidence
- **Decision owner:** Project owner
- **Confirmation:** Current implementation is documented at the project owner’s request; historical team approval is not recorded.

> This record documents the technology in the current implementation. The options and rationale below are a retrospective comparison, not a claim that the original project formally evaluated them.

## Contents

- [Context and problem statement](#context-and-problem-statement)
- [Decision drivers](#decision-drivers)
- [Options considered](#options-considered)
- [Decision outcome](#decision-outcome)
- [Consequences](#consequences)
- [Evidence and realization](#evidence-and-realization)
- [Review triggers](#review-triggers)
- [References](#references)

## Context and problem statement

The Compose stack runs PostgreSQL 18 and configures SonarQube to connect to it. A named volume retains the database across container replacement. SonarQube owns its internal schema; this project documents the database boundary without treating vendor tables as an application contract. The original database-choice record was not found.

The scope of this decision is PostgreSQL as the external relational database for the local SonarQube Community Build service. The source confirms the implementation; its historical selection rationale and original option set are not recorded.

## Decision drivers

- Keep SonarQube analysis and project state persistent across container restarts.
- Run the database as an explicit service with health ordering and durable local storage.
- Avoid building an unsupported direct integration with SonarQube private tables.

## Options considered

### PostgreSQL service with a named volume

- **Benefits:** Matches the current Compose configuration and keeps database lifecycle explicit.
- **Costs and risks:** Requires local database operations, password handling, and backup planning.

### Another database supported by the exact SonarQube release

- **Benefits:** Could be considered if compatibility, operational ownership, and migration path are verified.
- **Costs and risks:** No alternative engine is configured or evaluated in this repository; compatibility must be checked against the deployed release.

### Disposable or embedded local state

- **Benefits:** Would reduce separate database setup.
- **Costs and risks:** Would not match the current persistent analysis-history workflow and would complicate restart and data-retention expectations.

## Decision outcome

Retain PostgreSQL 18 as the Compose database for SonarQube state. Treat SonarQube-owned tables as private implementation data and use supported server APIs and UI for access.

## Consequences

### Positive

- The database service has an explicit health check and persistent named storage.
- Container recreation can retain analysis history when operators use the documented non-destructive down command.

### Negative and risks

- This workstation stack has no verified backup/restore procedure or high-availability design.
- The default local password and published port require workstation-only exposure controls.

## Evidence and realization

- [compose.yaml](../../compose.yaml)
- [system-design.md](../system-design.md)
- [database-model.md](../database-model.md)
- [README.md](../../README.md)

## Review triggers

- Reconsider if the SonarQube version, supported database requirements, or deployment target changes.
- Before changing database engines or versions, document vendor-supported compatibility and verify an explicit backup/restore path.

## References

- [sonarqube README](../../README.md)
- [System Design](../system-design.md)
- [ADR practices](https://adr.github.io/ad-practices/)
- [ADR template guidance](https://adr.github.io/adr-templates/)

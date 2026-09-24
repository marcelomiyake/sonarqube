# ADR-0001: Keep the Local SonarQube Stack in Docker Compose

- **Status:** Implemented (retrospective)
- **Recorded:** 2026-09-25
- **Original decision date:** Unknown from repository evidence
- **Decision owner:** Project owner
- **Confirmation:** Current implementation is documented at the project owner's request; historical team approval is not recorded.

> This record describes the current local Compose deployment. The alternatives below are retrospective and do not claim that the original project formally evaluated them.

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

The repository provides a local SonarQube Community Build service backed by PostgreSQL 18. Five project repositories submit scanner analysis to this endpoint. The stack is a single-user development and verification environment, not a production service. Its named volumes preserve database, SonarQube data, extensions, and logs across container removal.

The decision is how to package and operate the SonarQube service for this local workflow while keeping scanner configuration in consumer repositories.

## Decision drivers

- Provide a reproducible local endpoint shared by the known project repositories.
- Keep server and database lifecycle configuration in one repository.
- Preserve local analysis history when containers stop or are recreated.
- Keep operational requirements within a workstation-scale proof of concept.

## Options considered

### Docker Compose with persistent named volumes

- **Benefits:** One manifest starts the server and database together, health-check ordering is explicit, and named volumes preserve state across container replacement.
- **Costs and risks:** Operators must manage workstation resources, port exposure, credentials, and local volume backups; the current Compose file does not set CPU or memory limits.

### Remote shared SonarQube service

- **Benefits:** Moves server resource use and persistence off workstations and can provide centralized access.
- **Costs and risks:** Requires an independently operated service, identity/access controls, network availability, credentials, and an ownership/backup model not present in this repository.

### Ephemeral local containers with disposable data

- **Benefits:** Easy to reset and avoids persistent local state.
- **Costs and risks:** Deletes project settings and analysis history during cleanup, undermining the current review workflow and potentially causing repeated setup.

This is a retrospective comparison; no repository evidence records a historical evaluation or formal approval process.

## Decision outcome

Keep the local SonarQube Community Build and PostgreSQL 18 services in Docker Compose with named volumes. Consumer repositories own scanner configuration and submit analyses to the local endpoint. Use `docker compose down` to remove containers while retaining state; reserve `docker compose down -v` for intentional deletion of persistent data.

## Consequences

### Positive

- The server, database, health dependency, published endpoint, and volume bindings are defined together.
- Stopping or recreating containers does not erase analysis history by default.
- Scanner clients remain owned by the repositories whose code they analyze.

### Negative and risks

- The single-user stack has no production HA, remote-access hardening, or backup/restore guarantee.
- The Compose configuration has no CPU or memory resource caps, so operators must account for host resource use.
- The published port and fallback local database password require workstation-only use and careful network exposure.
- Removing volumes permanently deletes local database state and SonarQube data, extensions, and logs.

## Evidence and realization

- The [Compose manifest](../../compose.yaml) defines `sonarqube:community`, `postgres:18`, health-check ordering, port 9000, and four named volumes.
- The [System Design](../system-design.md) describes consumers, contracts, persistence, security, and operations.
- The [project README](../../README.md) documents startup and cleanup commands, including the data-retention difference between `down` and `down -v`.
- The five known consumers and their scanner paths are recorded in the [contract catalog](../contracts/README.md).

## Review triggers

- Reconsider if SonarQube becomes a shared multi-user or network-accessible service.
- Add a backup/restore design before local analysis history becomes important operational data.
- Add and validate Compose CPU/memory limits if workstation contention requires explicit caps.
- Reconsider the topology if the project needs remote centralized analysis or managed service ownership.

## References

- [Local SonarQube README](../../README.md)
- [System Design](../system-design.md)
- [Docker Compose configuration](../../compose.yaml)
- [ADR practices](https://adr.github.io/ad-practices/)
- [ADR template guidance](https://adr.github.io/adr-templates/)

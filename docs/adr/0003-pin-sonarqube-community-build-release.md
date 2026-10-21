# ADR-0003: Pin the SonarQube Community Build Release

- **Status:** Accepted
- **Recorded:** 2026-10-21
- **Original decision date:** 2026-10-21
- **Decision owner(s):** Project owner
- **Supersedes:** None
- **Confirmation:** The project owner requested this update on 2026-10-21. It is implemented in [`compose.yaml`](../../compose.yaml).

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

Compose used the floating `sonarqube:community` tag. On 2026-10-21 that tag resolved to SonarQube Community Build 26.10.0.133078, with index digest `sha256:0b3408443fb6a311253de27941b10977c9b977fd53be2265947bfe14161308b8` and linux/amd64 manifest `sha256:4f3ff53a16b25e1707dada7204eb9f00145e3aed829fdcef42aad954a12941ea`. Because the tag floats, `docker compose pull` could upgrade the server and migrate the database schema without a change to this repository.

SonarSource's [database requirements](https://docs.sonarsource.com/sonarqube-community-build/server-installation/installing-the-database) list PostgreSQL "15 to 18 (starting in SonarQube Community Build 26.8)". The stack runs `postgres:18`, so [ADR-0002](0002-use-postgresql-for-sonarqube-state.md) remains valid. PostgreSQL 14 is no longer supported.

The question is how Compose should select the SonarQube image.

## Decision drivers

- Make the deployed SonarQube version explicit and reviewable in the repository.
- Avoid unplanned, irreversible schema migrations of local analysis history.
- Keep the database version inside the vendor-supported range for the deployed release.
- Keep receiving the publisher's rebuilt images for the same release, which may include patched base layers.

## Options considered

### Floating `community` tag

- **Benefits:** No manual version bumps; always pulls the latest Community Build.
- **Costs and risks:** The deployed version is implicit, upgrades and schema migrations can happen on any pull, and database compatibility is not checked before an upgrade.

### Exact release tag (`26.10.0.133078-community`)

- **Benefits:** The version is explicit in `compose.yaml`, upgrades are deliberate changes, and the database support range can be checked for each bump.
- **Costs and risks:** Operators must bump the tag manually. Docker Official Images may rebuild a release tag (for example, to update its base image), so the digest can change without a version change.

### Exact release tag plus digest

- **Benefits:** Fully immutable image selection.
- **Costs and risks:** Also blocks rebuilt images that patch the same release, and requires a digest update for every change. That is more maintenance than this single-user workstation stack needs.

## Decision outcome

Pin `sonarqube:26.10.0.133078-community` and keep `postgres:18`. The exact release tag makes upgrades deliberate and still lets patched rebuilds of the same release arrive. Digest pinning is unnecessary for this local, single-user stack.

## Consequences

### Positive

- The SonarQube version and its PostgreSQL support range are documented together.
- Upgrades require a reviewed `compose.yaml` change, so operators can back up the database first.

### Negative and risks

- New Community Builds, including security fixes in newer releases, arrive only when someone bumps the tag.
- Rebuilt images for the same release tag can still change the digest between pulls.
- Existing local volumes created by an older SonarQube version are migrated the first time this release starts against them. The migration is not reversible without a database backup.

## Evidence and realization

- [`compose.yaml`](../../compose.yaml) pins `sonarqube:26.10.0.133078-community` and `postgres:18`.
- On 2026-10-21, `docker buildx imagetools inspect sonarqube:26.10.0.133078-community` resolved to the same index digest as `sonarqube:community`. Its linux/amd64 manifest was `sha256:4f3ff53a…12941ea`.
- On 2026-10-21, the repository stack ran in an isolated Compose project with fresh volumes (`docker compose -p sonarqube-verify up -d`). `api/system/status` reported version `26.10.0.133078` and status `UP`. PostgreSQL reported `18.6`, and the logs showed no database errors. The [README upgrade procedure](../../README.md#upgrade-sonarqube) backup and restore commands completed against that stack (176 public tables before and after restore), and SonarQube returned to `UP`. The isolated project and its volumes were then removed.
- Not verified: migrating the operator's existing `sonarqube_*` volumes from the SonarQube version previously deployed. That version was not recorded in the repository.

## Review triggers

- Bump the tag when a newer Community Build is needed. Before bumping, check SonarSource's update path and database requirements, back up the database, and record the new version in this repository's docs.
- Re-evaluate `postgres:18` if a target release drops PostgreSQL 18 support, or before moving to a newer PostgreSQL major version. That change also requires a PostgreSQL data migration.
- Consider digest pinning if silent rebuilds of a release tag cause drift or verification problems.

## References

- [ADR-0001: Keep the Local SonarQube Stack in Docker Compose](0001-keep-sonarqube-local-with-compose.md)
- [ADR-0002: Use PostgreSQL for SonarQube State](0002-use-postgresql-for-sonarqube-state.md)
- [System Design](../system-design.md)
- [Project README](../../README.md)
- [SonarQube Community Build database requirements](https://docs.sonarsource.com/sonarqube-community-build/server-installation/installing-the-database)
- [SonarQube Community Build update guide](https://docs.sonarsource.com/sonarqube-community-build/server-update-and-maintenance/update)
- [Docker Hub `sonarqube` tags](https://hub.docker.com/_/sonarqube/tags)

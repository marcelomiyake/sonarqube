# SonarQube database model boundary

This repository deploys SonarQube Community Build with PostgreSQL, but it does not define or migrate SonarQube's internal tables. The vendor application owns that schema and may change it between releases.

## Repository-owned database artifacts

There are no repository-owned table definitions, columns, or migrations in this project. The Compose file configures a PostgreSQL database named `sonarqube` for the SonarQube server; its persisted content includes project configuration and analysis history managed by SonarQube itself. This page intentionally does not infer the vendor's private table layout from a running instance.

- **Repository operator:** `sonarqube`, via [`compose.yaml`](../compose.yaml).
- **Schema owner:** SonarSource / the SonarQube server version deployed by Compose.
- **Application consumers:** `sonarqube` / the Compose-managed SonarQube server is the only database client in this repository. Scanner clients use SonarQube's HTTP contract and do not connect to PostgreSQL.
- **Versioned local contract:** PostgreSQL connection variables and named-volume persistence in [`compose.yaml`](../compose.yaml).

Use the vendor's supported upgrade and backup procedures for the deployed release. Do not query or modify internal tables directly as an application integration.

## Related documentation

- [Contract catalog](contracts/README.md)
- [Project README](../README.md)
- [Documentation index](README.md)

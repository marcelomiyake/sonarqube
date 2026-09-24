# Architectural Decision Records

This index records the significant architectural choices for the local SonarQube stack. Each ADR covers one decision and links to its implementation evidence.

## Records

- [ADR-0001: Keep the Local SonarQube Stack in Docker Compose](0001-keep-sonarqube-local-with-compose.md)
- [ADR-0002: Use PostgreSQL for SonarQube State](0002-use-postgresql-for-sonarqube-state.md)

## Maintaining this log

New records use the [ADR template](../templates/adr.template.md). Keep one decision per file, number records sequentially, and add a new ADR when an accepted or implemented decision changes. Do not rewrite historical outcomes. Retrospective records distinguish known implementation evidence from unknown original decision dates or approval history.

This Markdown index has no separate software build, deploy, use, or undeploy lifecycle; see the [project README](../../README.md) for application operations.

## AI development disclaimer

> **AI development disclaimer:** This project was built entirely with GPT-6 Luna at Max effort as a proof of concept exploring how low-cost AI plans can be useful when paired with disciplined harness and loop engineering. This is project-owner attribution; repository contents do not independently verify runtime model metadata. Review AI-generated design and code before relying on them.

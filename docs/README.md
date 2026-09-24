# Documentation index

Use this index to find the repository overview, agent guidance, architecture, contracts, operational instructions, verification records, and reusable templates. This static Markdown index has no application build, deployment, or undeployment lifecycle.

## Contents

- [Document responsibilities](#document-responsibilities)
- [Project and component guides](#project-and-component-guides)
- [Agent instructions](#agent-instructions)
- [Architecture and contracts](#architecture-and-contracts)
- [Standards and reusable templates](#standards-and-reusable-templates)
- [Applying the standard](#applying-the-standard)
- [AI development disclaimer](#ai-development-disclaimer)

## Document responsibilities

- This index ([docs/README.md](README.md)) links every project Markdown document by purpose.
- Root and component `README.md` files help people build, deploy, use, and contribute.
- `AGENTS.md` files give scoped instructions to coding agents.
- Architecture and contract records identify system boundaries, owners, producers, consumers, and authoritative sources.
- Operational and verification documents explain applicable lifecycle commands and recorded evidence.

## Project and component guides

- [README.md](../README.md) — Root or component README
- [docs/README.md](README.md) — Documentation index

## Agent instructions

- [docs/agent-documentation-hooks.md](agent-documentation-hooks.md) — Codex hook recommendation for post-agent documentation sync

- [AGENTS.md](../AGENTS.md) — Scoped agent guidance


- [CLAUDE.md](../CLAUDE.md) — Single-line import of scoped `AGENTS.md` guidance

## Architecture and contracts

- [docs/contracts/README.md](contracts/README.md) — API/data contract catalog
- [docs/database-model.md](database-model.md) — database tables, columns, ownership, and descriptions
- [docs/system-design.md](system-design.md) — implemented local SonarQube architecture and analysis lifecycle

- [docs/adr/README.md](adr/README.md) — Architectural Decision Records index and maintenance guidance
- [docs/adr/0001-keep-sonarqube-local-with-compose.md](adr/0001-keep-sonarqube-local-with-compose.md) — ADR-0001: Keep the Local SonarQube Stack in Docker Compose
- [docs/adr/0002-use-postgresql-for-sonarqube-state.md](adr/0002-use-postgresql-for-sonarqube-state.md) — ADR-0002: Use PostgreSQL for SonarQube State

## Standards and reusable templates

- [docs/documentation-standard.md](documentation-standard.md) — Documentation standard
- [docs/templates/adr.template.md](templates/adr.template.md) — Reusable Markdown ADR template
- [docs/templates/agents.template.md](templates/agents.template.md) — Reusable Markdown template
- [docs/templates/api-contract.template.md](templates/api-contract.template.md) — Reusable Markdown template
- [docs/templates/component-readme.template.md](templates/component-readme.template.md) — Reusable Markdown template
- [docs/templates/design-notes.template.md](templates/design-notes.template.md) — Reusable Markdown template
- [docs/templates/design-report.template.md](templates/design-report.template.md) — Reusable Markdown template
- [docs/templates/general-document.template.md](templates/general-document.template.md) — Reusable Markdown template
- [docs/templates/operations-guide.template.md](templates/operations-guide.template.md) — Reusable Markdown template
- [docs/templates/root-readme.template.md](templates/root-readme.template.md) — Reusable Markdown template
- [docs/templates/system-design.template.md](templates/system-design.template.md) — Reusable Markdown template
- [docs/templates/verification-record.template.md](templates/verification-record.template.md) — Reusable Markdown template

## Applying the standard

- [Documentation standard](documentation-standard.md)
- [Component README template](templates/component-readme.template.md)


## AI development disclaimer

> **AI development disclaimer:** This project was built entirely with GPT-6 Luna at Max effort as a proof of concept exploring how low-cost AI plans can be useful when paired with disciplined harness and loop engineering. This is project-owner attribution; repository contents do not independently verify runtime model metadata. Review AI-generated design and code before relying on them.

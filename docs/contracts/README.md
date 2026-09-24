# SonarQube integration contract catalog

This repository runs a local SonarQube Community Build instance; it does not own SonarQube's HTTP API. SonarSource owns that service contract. This catalog maps the known local scanner consumers across the project repositories.

> Documentation: [project index](../README.md) · [repository overview](../../README.md)

## Contents

- [Contract and consumers](#contract-and-consumers)
- [Use and compatibility](#use-and-compatibility)
- [Related documentation](#related-documentation)
- [Document lifecycle](#document-lifecycle)
- [AI development disclaimer](#ai-development-disclaimer)

## Contract and consumers

| Contract | Contract owner | Local instance operator | Known consumers |
| --- | --- | --- | --- |
| SonarQube Web API and scanner submission protocol | SonarSource; see the [official Web API](https://docs.sonarsource.com/sonarqube-server/extension-guide/web-api/) and scanner documentation | [`sonarqube`](https://github.com/marcelomiyake/sonarqube) repository; `compose.yaml` | [`notification-system`](https://github.com/marcelomiyake/notification-system/tree/main/.sonar) / `.sonar`; [`opentube`](https://github.com/marcelomiyake/opentube/tree/main/scripts) / `scripts/sonar-scan-all.sh`; [`search-autocomplete-system`](https://github.com/marcelomiyake/search-autocomplete-system/tree/main/scripts) / `scripts/sonar-scan.sh`; [`url-shortener`](https://github.com/marcelomiyake/url-shortener/tree/main) / `scripts/sonar-scan.sh`; [`web-crawler`](https://github.com/marcelomiyake/web-crawler/tree/main/.sonar) / `.sonar`. |
| SonarQube MCP connection | SonarSource MCP server; local Codex configuration in the [project README](../README.md) | [`sonarqube`](https://github.com/marcelomiyake/sonarqube) / local Compose services | The local Codex MCP client. The token stays in local configuration and is not a repository artifact. |

## Use and compatibility

Scanner clients send analysis reports to the configured `SONAR_HOST_URL` and authenticate using project-scoped or configured tokens. The server's Web API and plugin/analyzer compatibility can change independently; check the vendor documentation before changing scanner integration. No API credentials belong in this repository.

## Related documentation

- [Database model boundary](../database-model.md)

- [Project README](../../README.md)
- [Documentation index](../README.md)

## Document lifecycle

This contract catalog is maintained as Markdown and links to the implementation-owned interface definitions. It has no independent software build, deployment, or undeployment lifecycle. Review it when its linked contracts or consumers change.


## AI development disclaimer

> **AI development disclaimer:** This project was built entirely with GPT-6 Luna at Max effort as a proof of concept exploring how low-cost AI plans can be useful when paired with disciplined harness and loop engineering. This is project-owner attribution; repository contents do not independently verify runtime model metadata. Review AI-generated design and code before relying on them.

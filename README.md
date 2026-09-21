# SOC Investigation Agent Workshop

A hands-on guide for building and operating an Azure AI Foundry Investigation Agent that investigates Microsoft Sentinel and Microsoft Defender for Endpoint (MDE) telemetry. PentAGI is included only as an authorized red-team telemetry generator; it is not part of the investigation runtime.

## Workshop outcome

By the end of this workshop, you will have:

- A PentAGI red-team lab configured with the current Azure OpenAI-compatible provider.
- Two secured MCP connections: Sentinel and MDE.
- A published, investigation-only Foundry agent.
- A repeatable method to inspect traces and correct failed KQL queries.

## Architecture

```text
PentAGI (authorized red-team simulation)
                 |
                 v
      Linux endpoint + MDE sensor
                 |
                 v
Microsoft Sentinel / Log Analytics workspace
                 |
                 v
Azure AI Foundry Investigation Agent
         |                       |
         v                       v
   SentinelMCP                MdeMCP
```

## Workshop modules

| # | Module | Goal | Est. time |
|---|---|---|---|
| 01 | [Environment](./1-environment.md) | Understand the current lab components and required access. | 10 min |
| 02 | [PentAGI Red Team Setup](./2-pentagi.md) | Configure PentAGI and choose the correct execution mode. | 15 min |
| 03 | [MCP Connections](./3-mcp-connections.md) | Add SentinelMCP and MdeMCP safely in Foundry. | 10 min |
| 04 | [Investigation Agent](./4-investigation-agent.md) | Create, publish, and test the read-only Investigation Agent. | 15 min |
| 05 | [Traces and Troubleshooting](./5-traces-troubleshooting.md) | Validate tool calls and recover from common investigation failures. | 10 min |

## Prerequisites

- Access to Azure AI Foundry project `lab-project-001`.
- Azure permissions to use the project connections.
- Sentinel read access to workspace `lab-law-001`.
- Docker Desktop for the PentAGI module.
- Authorization for every PentAGI target and test scope.

## Screenshots

Use the checklist in [assets/README.md](./assets/README.md). The workshop remains usable without screenshots; each image is optional supporting context, not a required source of truth.

## Important boundaries

- The Investigation Agent is read-only: it must not isolate devices, run scans, or delete files.
- PentAGI is a red-team/pentest platform and must only target systems in an approved scope.
- Never put API keys, client secrets, or MCP bearer tokens in prompts, screenshots, or git.

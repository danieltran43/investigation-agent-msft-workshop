# 01 — Environment

## Objective

Verify the lab components and understand which system owns each responsibility before creating an agent.

## Current lab configuration

| Component | Current configuration | Responsibility |
|---|---|---|
| Foundry project | `lab-project-001` | Hosts the Investigation Agent and MCP connections. |
| Sentinel workspace | `lab-law-001` | Stores security telemetry and supports KQL investigation. |
| Sentinel MCP | `https://mcp-sentinel.gentleflower-524cd753.eastus2.azurecontainerapps.io/mcp` | Exposes Sentinel/Log Analytics tools. |
| MDE MCP | `https://mcp-mde.gentleflower-524cd753.eastus2.azurecontainerapps.io/mcp` | Exposes Microsoft Defender for Endpoint tools. |
| PentAGI | `D:\SOC-Lab\pentagi` | Red-team lab simulator only. |

## Required access

- Foundry project access to create an agent and attach existing connections.
- Sentinel/Log Analytics read access for the principal used by SentinelMCP.
- MDE read permissions for the principal used by MdeMCP.
- Azure CLI access only when diagnosing deployment, connection, or RBAC issues.

## Do not mix credentials

| Credential | Where it belongs |
|---|---|
| `MCP_API_KEY` | The corresponding MCP Container App and Foundry MCP connection. |
| Azure tenant/client secret | The relevant MCP service principal, never PentAGI. |
| `LLM_SERVER_KEY` / `EMBEDDING_KEY` | PentAGI `.env`. |
| Foundry model deployment | Azure AI Foundry agent configuration. |

## Verification

1. Open Azure AI Foundry and select `lab-project-001`.
2. Confirm the Sentinel workspace name is `lab-law-001`.
3. Confirm the two MCP URLs end in `/mcp`.
4. Do not use `/health` as a Foundry MCP connection URL; it is a liveness endpoint only.

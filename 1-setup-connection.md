# 01 — Setup Connection — Connect SentinelMCP and MdeMCP

## Objective

Attach the two existing MCP servers to the Foundry project without exposing secrets.

## Prerequisite

```Endpoints
SentinelMCP: https://mcp-sentinel.gentleflower-524cd753.eastus2.azurecontainerapps.io/mcp
MdeMCP:      https://mcp-mde.gentleflower-524cd753.eastus2.azurecontainerapps.io/mcp
```

Information only. You will be able to select these MCPs as an option during the lab.

## Request Access To Your Environement
1. Scan the QR code
![QR Code To Access Lab Environment](./assets/0-vertexia-lab-request.png)
2. From the QR code, you will received a link to request access to the lab environment, input your Microsoft account
![Input Your Microsoft Account](./assets/01-input-email.png)
3. Complete your authentication and MFA
![Complete your authentication and MFA](./assets/02-complete-your-authentication.png)
4. Now you are able to request access to the lab environment, click Next
![Request Access To The Lab Environment](./assets/03-request-your-accesss.png)
5. No need to defined anything for now, just click on Next to continue the the request
![Skip any clarification](./assets/04-skip-to-continue.png)

## Foundry UI steps

1. Open Azure AI Foundry and select `lab-project-001`.
2. Open **Build → Agents**, then open or create the Investigation Agent.
3. Under **Tools**, select **Add → Model Context Protocol (MCP)**.
4. Add `SentinelMCP` with the Sentinel endpoint and bearer credential.
5. Add `MdeMCP` with the MDE endpoint and bearer credential.
6. Save, reconnect/refresh each connection, and confirm tool discovery succeeds.

Choose **Model Context Protocol (MCP)** from the Custom tool catalog:

![Foundry Custom tool catalog with Model Context Protocol selected](./assets/03-select-mcp-tool.png)

Use the following connection shape for MdeMCP: the endpoint ends with `/mcp`, authentication is **Key-based**, the credential name is `Authorization`, and the secret value is a bearer token. The value is masked in the image and must remain masked in documentation.

![MdeMCP connection configured with a masked bearer credential](./assets/03-mde-mcp-connection.png)

After connection, verify the project **Tools** list contains both `SentinelMCP` and `MdeMCP` as **Model Context Protocol (MCP)** tools:

![Foundry Tools list showing SentinelMCP and MdeMCP](./assets/03-foundry-mcp-tools.png)

## Expected tool categories

| MCP | Expected investigation tools |
|---|---|
| SentinelMCP | Workspace discovery, table/schema discovery, KQL query, alert/incident lookup. |
| MdeMCP | Machine inventory, machine details, related alerts, endpoint telemetry lookup. |

## Validation

- Connection state must not show `401`, `403`, or `tools/list failed`.
- If a key changed, update both the Container App secret and the Foundry connection, then reconnect.
- Test the Investigation Agent directly before wiring it through any Orchestrator/A2A flow.

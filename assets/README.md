# Screenshot checklist

Add screenshots here using the file names below. Blur or crop API keys, bearer tokens, client secrets, tenant IDs not needed for the guide, usernames, and unrelated browser tabs.

| Status | File name | What to capture | Used by |
|---|---|---|---|
| Added | `02-pentagi-new-flow.png` | PentAGI New Flow page showing both modes; the screenshot has Assistant selected, while Module 02 recommends Automation for an approved engagement. | Module 02 |
| Added | `03-select-mcp-tool.png` | Custom tool catalog with Model Context Protocol selected. | Module 03 |
| Added | `03-mde-mcp-connection.png` | MdeMCP endpoint, Key-based auth, and masked `Authorization` bearer credential. | Module 03 |
| Added | `03-foundry-mcp-tools.png` | Foundry project Tools list showing SentinelMCP and MdeMCP as MCP tools. | Module 03 |
| Added | `04-foundry-agents-list.png` | Foundry Agents page and New agent menu. | Module 04 |
| Added | `04-investigation-agent.png` | Investigation Agent editor with model, instructions, and Tools panel. | Module 04 |
| Added | `04-agent-add-tools.png` | Investigation Agent Add tools menu showing existing tool/toolbox entries. | Module 04 |
| Added | `04-agent-test-result.png` | Successful Investigation Agent chat run returning a severity-sorted Sentinel alert list with both MCP tools attached. | Module 04 |
| Added | `05-foundry-trace.png` | Foundry Trajectories trace showing Investigation Agent calls and Sentinel MCP `Execute Tool` events with the Input + Output pane. | Module 05 |

Reference an image from a workshop module only after adding it:

```markdown
![Foundry MCP tools](./assets/03-foundry-mcp-tools.png)
```

Do not capture or commit `.env` files, access tokens, API keys, MCP bearer tokens, client secrets, or raw production telemetry.

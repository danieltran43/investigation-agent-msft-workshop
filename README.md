# SOC Investigation Agent Workshop
 
A hands-on guide for building and operating an Azure AI Foundry Investigation Agent that investigates Microsoft Sentinel and Microsoft Defender for Endpoint (MDE) telemetry.
 
## Workshop outcome
 
By the end of this workshop, you will have:
 
- Requested and been granted access to the shared lab environment, with MFA configured for the lab tenant.
- Signed in to Azure AI Foundry and created an agent with the `SentinelMCP` and `MdeMCP` tools attached.
- A published, investigation-only Foundry agent with a verified basic and deep-investigation run.
- A repeatable method to inspect traces and correct failed KQL queries.
 
## Architecture
 
```text
You
|
v
Lab access request + MFA (lab tenant)
|
v
Azure AI Foundry (lab-project-001)
|
v
Investigation Agent
|                 |
v                 v
SentinelMCP       MdeMCP
|                 |
v                 v
Microsoft Sentinel  Microsoft Defender for Endpoint
(workspace lab-law-001)
```
 
## Workshop modules
 
| # | Module | Goal | Est. time |
|---|---|---|---|
| 01 | [Setup Connection](./1-setup-connection.md) | Request lab access, set up MFA, sign in to Foundry, create the agent, and attach SentinelMCP/MdeMCP. | 20 min |
| 02 | [Investigation Agent](./2-investigation-agent.md) | Paste the agent instructions, publish the agent, and run basic and deep-investigation validations. | 15 min |
| 03 | [Traces and Troubleshooting](./3-traces-troubleshooting.md) | Validate tool calls and recover from common investigation failures. | 10 min |
 
## Prerequisites
 
- A Microsoft account you can use to request access to the lab environment.
- A phone with the Microsoft Authenticator app (or willingness to install it) for MFA setup.
- Access to Azure AI Foundry project `lab-project-001` once the lab request is approved.
- Sentinel read access to workspace `lab-law-001`.
 
## Screenshots
 
Use the checklist in [assets/README.md](./assets/README.md). The workshop remains usable without screenshots; each image is optional supporting context, not a required source of truth.
 
## Important boundaries
 
- The Investigation Agent is read-only: it must not isolate devices, run scans, or delete files.
- Never put API keys, client secrets, or MCP bearer tokens in prompts, screenshots, or git.

# PentAGI Lab Assessment Flow and Strategy

## Purpose

This document describes a time-boxed, authorized security assessment of the controlled Azure lab. The objective is to demonstrate how an attacker may reach and validate application-level weaknesses, while giving Microsoft Defender for Endpoint (MDE), Microsoft Sentinel, and the Azure AI Foundry Investigation Agent enough telemetry to reconstruct the activity.

The assessment is designed for a disposable lab only. It must not be used against production systems or assets outside the approved scope.

## Lab architecture

```text
PentAGI Automation
        |
        | Authorized HTTP assessment traffic
        v
Azure public IP: 74.249.50.100:5173
        |
        v
Azure NSG: inbound TCP/5173, source allow-listed or lab-approved
        |
        v
Ubuntu VM: lab-mde
        |
        +-- UFW host firewall
        +-- systemd: next-rsc-lab.service
        |      User: next-rsc-lab
        |      NoNewPrivileges=true
        |      PrivateTmp=true
        |
        +-- Next.js application on 0.0.0.0:5173
        |      /debug
        |      /admin
        |      /api/lab-upload
        |      /lab-fixtures/
        |      permissive demo CORS
        |
        +-- MDE Linux sensor
                |
                +--> Microsoft Defender for Endpoint
                |       alerts, incidents, device timeline
                |
                +--> Microsoft Sentinel workspace lab-law-001

Azure AI Foundry project lab-project-001
        |
        +-- Investigation Agent
                |
                +-- SentinelMCP --> Sentinel read-only tools
                +-- MdeMCP      --> MDE read-only telemetry tools
```

PentAGI generates assessment traffic and evidence. It is not the investigation system. The Foundry Investigation Agent correlates the resulting Sentinel and MDE data through the two MCP connections.

## Assessment flow

### Phase 0 — Scope and readiness

Confirm the target URL, approved source address, test window, and disposable-lab status. Verify that TCP `5173` is reachable before starting the flow. If the connection fails twice with a short timeout, stop and report the target as unreachable. Do not scan alternate ports or continue retrying.

### Phase 1 — Minimal fingerprinting

Perform a small number of requests to identify the actual server, framework, version indicators, and visible routes. Do not infer the framework from the port number. Once the framework is positively identified, discard incompatible framework hypotheses and do not test unrelated CVEs.

Expected evidence includes:

- HTTP status and response headers
- framework or runtime indicators
- relevant application routes
- authentication and authorization signals
- exposed debug or lab-only fixture pages

### Phase 2 — Highest-confidence validation

Select one likely attack path based on the observed fingerprint and application behavior. Validate only the minimum necessary request sequence.

If code execution is suspected, use only harmless proof actions such as:

- `id`
- `whoami`
- `hostname`
- creation and verification of one temporary marker under `/tmp`

The proof must record the execution user, timestamp, process context, and response evidence. Once code execution is confirmed, stop exploitation and move directly to reporting.

### Phase 3 — Known application fixtures

Check the intentionally weak lab functionality:

- debug information disclosure
- fake administrative credentials
- public fake tokens or fixture files
- unauthenticated text upload behavior
- permissive CORS behavior

Use fake values only. Do not upload executable content, install packages, alter production-like data, or attempt persistence.

### Phase 4 — Read-only host security review

If an application execution context is available, perform a bounded read-only review of:

- UID/GID and process identity
- Linux capabilities
- sudo policy visibility
- SUID/SGID candidates
- systemd units and timers
- cron configuration and permissions
- service isolation controls such as `NoNewPrivileges`

After two independent checks show no viable privilege-escalation path, close the branch as not demonstrated. Never execute a root command or modify privileged files.

### Phase 5 — Telemetry correlation

Record the assessment timestamp and correlate:

- web requests and response codes
- process creation and child interpreters
- file creation or modification events
- network connection attempts
- execution user and parent process
- MDE alerts, incidents, and device timeline events
- related Sentinel records in `lab-law-001`

The goal is to compare raw activity with what the defensive systems actually detected. An event in the timeline is not automatically an alert or incident.

### Phase 6 — Reporting and stop

Stop when one high-confidence finding is confirmed, when the approved time budget expires, or when the target becomes unreachable. The final report should separate confirmed, not-demonstrated, and informational findings.

## PentAGI execution strategy

Use Automation mode with one sequential task. Give PentAGI the target and high-level hints, but do not provide a large attack checklist that encourages unnecessary decomposition.

Recommended constraints:

1. Maximum run time: 25 minutes.
2. One active task and no automatic subtask expansion.
3. No parallel agents.
4. No broad web research or long wordlists.
5. Maximum two connection attempts for an unavailable target.
6. Maximum one validation attempt and one retry per hypothesis.
7. No repeated checks using another tool, shell, interpreter, encoding, or protocol.
8. Stop after the first confirmed high-impact finding.
9. Produce the report immediately after the stop condition.

The following rules prevent the most common PentAGI loops:

```text
Do not infer the framework from the port number.
Do not continue testing incompatible CVEs after fingerprinting.
Treat a negative result as final unless new evidence changes the hypothesis.
Do not scan alternate ports or wait for a service to recover.
Do not repeat the same check with another tool or interpreter.
Close a privilege-escalation branch after two negative checks.
Do not search for alternative root, persistence, malware, C2, or lateral-movement paths.
```

## Defensive observation path

```text
PentAGI request
    -> Azure NSG / UFW
    -> Next.js process
    -> child process or file event, if any
    -> MDE sensor
    -> Defender alerts and device timeline
    -> Sentinel workspace
    -> Foundry Investigation Agent
    -> SentinelMCP + MdeMCP correlation
    -> analyst report
```

MDE may record process, file, and network telemetry even when no formal alert is generated. Alert creation depends on the behavior, process lineage, signatures, policy, and Defender detection logic. Harmless markers are useful for validating telemetry but are not expected to produce a high-severity alert by themselves.

## Investigation tools

### SentinelMCP

The investigation agent should use read-only tools such as:

- `list_workspaces`
- `search_tables`
- `get_table_schema`
- `query_lake`
- alert and incident lookup tools, when exposed by the MCP server

The agent should retrieve the table schema before writing KQL, use bounded time windows, and correlate results across more than one data source when possible.

### MdeMCP

The investigation agent should use read-only tools such as:

- `list_machines`
- `get_machine`
- `get_machine_related_alerts`
- process telemetry queries
- network telemetry queries
- file event queries

Response actions such as isolation, deletion, remediation, or scan initiation are outside this investigation workflow.

## Stop conditions

Stop immediately when any of the following occurs:

- harmless code execution is confirmed;
- a high-impact authorization or file-read issue is confirmed;
- a root path is discovered;
- the target becomes unavailable after two short attempts;
- the 25-minute time budget is reached;
- the next action would require malware, persistence, C2, credential dumping, lateral movement, or destructive behavior.

## Report format

```text
Finding:
Evidence:
Execution context:
Impact:
MDE telemetry:
Sentinel correlation:
Alert likelihood:
Remediation:
Confidence:
```

The report must not claim that an exploit, root access, malware execution, or alert was confirmed unless the trace and defensive telemetry contain direct supporting evidence.

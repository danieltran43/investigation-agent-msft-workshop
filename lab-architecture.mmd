%%{init: {'theme':'base','themeVariables': {'fontFamily':'Segoe UI','primaryTextColor':'#172033','lineColor':'#64748b','clusterBkg':'#f8fafc','clusterBorder':'#94a3b8'}}}%%
flowchart LR
    User["Pentester / Client\nAuthorized source IP"]:::actor
    PentAGI["PentAGI\nAutomation / Assistant\nAzure OpenAI-compatible LLM"]:::red

    subgraph AZ["Azure subscription / lab VNet"]
      NSG["NSG: inbound TCP 5173\nallow-list source IP\n(no public all-in rule)"]:::azure
      subgraph VM["Ubuntu VM: 74.249.50.100"]
        UFW["UFW host firewall\nTCP 5173"]:::firewall
        SVC["systemd: next-rsc-lab.service\nUser=next-rsc-lab\nNoNewPrivileges=true"]:::service
        APP["Next.js 15.2.5 / React 19.2.0\n0.0.0.0:5173\n/opt/next-rsc-lab"]:::app
        FIX["Intentional lab fixtures\ndebug disclosure\nadmin fake credentials\nunauthenticated text upload\nCORS: *\npublic fake tokens/files"]:::weak
        SAFE["Safety boundary\nnon-root service\nno malware / no persistence / no C2\nNoNewPrivileges + PrivateTmp"]:::safe
        MDEAgent["MDE Linux sensor\nprocess / file / network telemetry"]:::mde
      end
      Defender["Microsoft Defender for Endpoint\nalerts, incidents, device timeline"]:::mde
      Sentinel["Microsoft Sentinel\nworkspace: lab-law-001\nLog Analytics / KQL"]:::sentinel
    end

    subgraph FOUND["Azure AI Foundry: lab-project-001"]
      IA["Investigation Agent\nread-only correlation\nno response actions"]:::foundry
      SMCP["SentinelMCP\nread tools"]:::mcp
      MMCP["MdeMCP\nread telemetry tools"]:::mcp
    end

    User -->|HTTP 5173| NSG
    PentAGI -->|authorized test traffic| NSG
    NSG --> UFW --> SVC --> APP
    APP --> FIX
    SVC --> SAFE
    APP -. process/file/network events .-> MDEAgent
    MDEAgent -->|telemetry| Defender
    Defender -->|alerts/incidents/events| Sentinel
    IA --> SMCP -->|read-only queries| Sentinel
    IA --> MMCP -->|read-only device/process/file/network| Defender
    PentAGI -. test results / evidence .-> IA

    classDef actor fill:#e0f2fe,stroke:#0284c7,stroke-width:2px;
    classDef red fill:#fee2e2,stroke:#dc2626,stroke-width:2px;
    classDef azure fill:#dbeafe,stroke:#2563eb,stroke-width:2px;
    classDef firewall fill:#fef3c7,stroke:#d97706,stroke-width:2px;
    classDef service fill:#ede9fe,stroke:#7c3aed,stroke-width:2px;
    classDef app fill:#dcfce7,stroke:#16a34a,stroke-width:2px;
    classDef weak fill:#ffedd5,stroke:#ea580c,stroke-width:2px;
    classDef safe fill:#ecfccb,stroke:#65a30d,stroke-width:2px;
    classDef mde fill:#d1fae5,stroke:#059669,stroke-width:2px;
    classDef sentinel fill:#cffafe,stroke:#0891b2,stroke-width:2px;
    classDef foundry fill:#f3e8ff,stroke:#9333ea,stroke-width:2px;
    classDef mcp fill:#fae8ff,stroke:#c026d3,stroke-width:2px;

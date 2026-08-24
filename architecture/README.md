# Wazuh Lab Architecture

The lab follows an endpoint-to-manager monitoring model:

```text
Kali Linux / Controlled Activity
            ↓
     Windows Endpoint
       Wazuh Agent
            ↓
      Wazuh Manager
            ↓
     Wazuh Dashboard
            ↓
  Analyst Investigation
            ↓
Threat Intelligence Enrichment
```

The complete architecture diagram should be stored in this directory as `wazuh-lab-architecture.svg` or `.png`.
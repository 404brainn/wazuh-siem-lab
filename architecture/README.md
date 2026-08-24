# Wazuh Lab Architecture

The completed lab follows an endpoint-to-manager monitoring model documented in the project report:

```text
Kali Linux (Attacker)
        ↓
Windows 11 Endpoint (Wazuh Agent)
        ↓
Ubuntu Wazuh Manager
        ↓
Wazuh Dashboard / Threat Hunting
```

The lab uses Kali Linux for controlled reconnaissance simulation, Windows 11 as the monitored endpoint, and Ubuntu Server as the Wazuh Manager/SIEM.

The report does not document a completed external threat-intelligence enrichment stage, so VirusTotal is not shown as part of the completed architecture.
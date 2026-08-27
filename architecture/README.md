# Wazuh SOC Lab Architecture

![Wazuh SOC Lab Architecture](wazuh-soc-architecture.svg)

The completed lab uses three systems with two distinct flows.

## Attack Flow

```text
Kali Linux → Controlled Nmap reconnaissance → Windows 11 endpoint
```

Kali Linux was used only to generate authorized test activity against the monitored Windows endpoint.

## Security Telemetry Flow

```text
Windows Wazuh Agent ─┐
                     ├→ Wazuh Manager → Wazuh Indexer → Dashboard / Threat Hunting
Kali Wazuh Agent ────┘
```

The Windows endpoint provided Security Event, integrity-monitoring, and Windows Firewall telemetry. The Wazuh Manager processed events and correlation rules, while the Indexer and Dashboard were used for search, alert review, and threat hunting.

## Lab Systems

| System | Role | IP Address |
|---|---|---:|
| Ubuntu Server | Wazuh Manager / SIEM | `192.168.1.74` |
| Windows 11 | Monitored endpoint | `192.168.1.70` |
| Kali Linux | Controlled attack simulation | `192.168.1.71` |

VirusTotal enrichment, Sysmon integration, custom rules, and automated response were not part of the completed architecture and are therefore not shown as implemented components.

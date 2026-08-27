# Wazuh SOC Lab Architecture

![Wazuh SOC Lab Architecture](wazuh-soc-architecture.svg)

This lab uses three systems: Ubuntu Server, Windows 11, and Kali Linux.

## Test Traffic

```text
Kali Linux → Controlled Nmap scan → Windows 11
```

I used Kali to generate authorized test traffic against the Windows machine.

## Event Flow

```text
Windows Wazuh Agent ─┐
                     ├→ Wazuh Manager → Wazuh Indexer → Dashboard / Threat Hunting
Kali Wazuh Agent ────┘
```

The Windows endpoint sends Security events, FIM events, and firewall logs to Wazuh. The manager processes the events and the dashboard is used to search and investigate them.

## Lab Systems

| System | Role | IP Address |
|---|---|---:|
| Ubuntu Server | Wazuh Manager / SIEM | `192.168.1.74` |
| Windows 11 | Monitored endpoint | `192.168.1.70` |
| Kali Linux | Test machine | `192.168.1.71` |

This diagram only shows the components that were used in the lab. Sysmon, VirusTotal, custom rules, and automated response were not part of the completed setup.

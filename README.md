# Threat-Intelligence Driven SOC Lab Using Wazuh SIEM

A hands-on Security Operations Center (SOC) and threat-hunting laboratory built with Wazuh, Windows 11, Ubuntu Server, and Kali Linux. The project demonstrates centralized endpoint monitoring, File Integrity Monitoring (FIM), Windows Firewall telemetry analysis, reconnaissance detection, SIEM event correlation, and MITRE ATT&CK-based threat hunting.

## Lab Environment

| System | Role |
|---|---|
| Ubuntu Server | Wazuh Manager / SIEM |
| Windows 11 | Monitored endpoint / Wazuh Agent |
| Kali Linux | Attacker / reconnaissance simulation |

## Architecture

```text
Kali Linux (Attacker)
        |
        v
Windows 11 Endpoint (Wazuh Agent)
        |
        v
Ubuntu Wazuh Manager
        |
        v
Wazuh Dashboard / Threat Hunting
```

## What Was Actually Implemented

- Wazuh Manager, Dashboard, and Indexer deployment
- Windows 11 and Kali Linux agent integration
- File Integrity Monitoring for a controlled Windows directory
- Registry integrity monitoring
- Windows Firewall telemetry ingestion
- Nmap SYN-scan simulation from Kali Linux
- Wazuh correlation rule validation for repeated firewall drop events
- MITRE ATT&CK threat hunting and event mapping

## Detection Use Cases

### 1. File Integrity Monitoring

Monitored `C:\Users\Public` for unauthorized file changes and validated checksum and registry-integrity alerts.

Observed rule IDs in the lab included **594**, **750**, and **550**.

### 2. Windows Firewall Telemetry

Configured Wazuh to ingest:

```text
C:\Windows\System32\LogFiles\Firewall\pfirewall.log
```

The firewall telemetry was used to identify suspicious network activity.

### 3. Network Scan Detection

Simulated reconnaissance from Kali Linux with:

```bash
nmap -sS 192.168.1.70
```

Wazuh generated a correlated firewall detection using **Rule ID 4151**, identifying multiple firewall drop events from the same source.

### 4. MITRE ATT&CK Threat Hunting

Used the Wazuh Threat Hunting module to investigate alerts, analyze correlated events, identify attack patterns, and map detections to ATT&CK techniques.

The report explicitly documents **T1046 – Network Service Scanning** for the reconnaissance scenario.

## Detection Workflow

```text
Endpoint / Attack Activity
          ↓
      Wazuh Agent
          ↓
      Wazuh Manager
          ↓
   FIM / Firewall Rules
          ↓
    Alert Correlation
          ↓
 Threat Hunting Investigation
          ↓
 MITRE ATT&CK Mapping
```

## Evidence

Screenshots extracted from the project report are stored under [`screenshots/`](screenshots/). They include agent connectivity, FIM alerts, firewall/reconnaissance detection, MITRE ATT&CK hunting results, and implementation challenges.

## Challenges Demonstrated

The report documents practical troubleshooting of:

- Agent version mismatch
- Kali agent connectivity problems
- Missing firewall telemetry
- XML `localfile` configuration errors

## Wazuh vs Splunk

This project is intentionally different from the Splunk lab. Wazuh demonstrates endpoint-centric monitoring, FIM, integrated agent visibility, firewall telemetry, rule-based correlation, and threat hunting. The Splunk project demonstrates SPL-driven detection engineering, search, dashboards, and alert workflows.

## Security Note

This repository contains only redacted configuration examples. Never commit passwords, API keys, tokens, private keys, or production configuration secrets. All attack simulations were performed in a controlled lab against systems used for authorized testing.

## Project Report

[View the complete Wazuh project report](reports/WAZUH-Project-Report.pdf)

## Repository Structure

```text
.
├── architecture/
├── comparison/
├── configuration/
├── detections/
├── reports/
├── rules/
├── screenshots/
└── README.md
```

## Author

**Ibrahim Khaleel M**
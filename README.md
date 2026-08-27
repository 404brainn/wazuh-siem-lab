# Wazuh-Based Security Monitoring and Threat Hunting SOC Lab

A hands-on Security Operations Center (SOC) laboratory built with Wazuh, Ubuntu Server, Windows 11, and Kali Linux. The project demonstrates centralized endpoint monitoring, File Integrity Monitoring (FIM), Windows Firewall telemetry analysis, reconnaissance detection, event correlation, and MITRE ATT&CK-based threat hunting.

> All testing was performed in a controlled private lab against systems used for authorized testing.

## Project Report

- [Detailed Markdown Report](reports/WAZUH-Project-Report.md)
- [Project Report PDF](reports/WAZUH_Project_Report.pdf)

## Lab Environment

| System | Role | IP Address | Version |
|---|---|---:|---|
| Ubuntu Server | Wazuh Manager / SIEM | `192.168.1.74` | Wazuh 4.14.5 |
| Windows 11 | Monitored endpoint / Wazuh Agent | `192.168.1.70` | Wazuh Agent 4.7.5 |
| Kali Linux | Attack simulation / Wazuh Agent | `192.168.1.71` | Wazuh Agent 4.14.5 |

## Architecture

![Wazuh SOC Lab Architecture](architecture/wazuh-soc-architecture.svg)

The architecture separates **attack traffic** from **security telemetry**. Kali Linux generates controlled reconnaissance traffic against the Windows endpoint, while Wazuh agents forward endpoint telemetry to the Wazuh Manager for processing, indexing, investigation, and threat hunting.

See [architecture documentation](architecture/README.md) for additional details.

## What Was Implemented

- Wazuh Manager, Indexer, and Dashboard deployment
- Windows 11 and Kali Linux agent integration
- File Integrity Monitoring for a controlled Windows directory
- Registry integrity monitoring
- Windows Security Event monitoring
- Windows Firewall telemetry ingestion
- Controlled Nmap SYN-scan simulation from Kali Linux
- Wazuh correlation validation for repeated firewall-drop events
- MITRE ATT&CK-based threat hunting and event classification

## Detection Use Cases

### File and Registry Integrity Monitoring

Monitored `C:\Users\Public` and observed integrity-related alerts, including:

- Rule **550** — Integrity checksum changed
- Rule **594** — Registry key integrity checksum changed
- Rule **750** — Registry value integrity checksum changed

Integrity alerts were treated as investigation leads and validated with endpoint context rather than automatically classified as malicious.

See [FIM detection notes](detections/file-integrity-monitoring.md).

### Windows Account and Authentication Monitoring

- Windows Event ID **4720** for local account creation
- Windows Event ID **4625** for failed logon activity
- Wazuh Rule **60122** observed for logon-failure activity

See [authentication monitoring notes](detections/authentication-monitoring.md).

### Network Scan Detection

A controlled SYN scan was performed from Kali Linux against the Windows endpoint:

```bash
nmap -sS 192.168.1.70
```

Windows Firewall telemetry was collected from:

```text
C:\Windows\System32\LogFiles\Firewall\pfirewall.log
```

Wazuh correlated repeated firewall-drop events and generated:

| Rule ID | Description | Level |
|---|---|---:|
| **4151** | Multiple Firewall drop events from same source | **10** |

A correlated alert can represent multiple underlying firewall events, so multiple scans do not necessarily produce one high-level alert per scan.

See [network scan detection notes](detections/network-scan-detection.md).

## Detection → Investigation → Response Workflow

1. **Detect** — Wazuh agents collect endpoint, integrity, authentication, and firewall telemetry.
2. **Investigate** — Review the affected endpoint, source, timestamp, rule ID, severity, and related events.
3. **Validate** — Determine whether the activity is expected lab activity or requires escalation.
4. **Respond** — Record findings and apply containment or remediation when justified. Automated response was not implemented in this lab.

## MITRE ATT&CK Threat Hunting

The reconnaissance scenario was reviewed in the context of:

**T1046 — Network Service Discovery**

The Wazuh Threat Hunting interface was used to review event context, severity, related activity, and ATT&CK classifications.

## Evidence

The repository contains an evidence guide in [`screenshots/`](screenshots/README.md). Add only screenshots that directly support a documented claim.

Recommended evidence:

- Agent connectivity
- Integrity alerts
- Rule 4151 firewall correlation alert
- MITRE ATT&CK Threat Hunting view

## Troubleshooting Demonstrated

| Challenge | Resolution |
|---|---|
| Agent version mismatch | Upgraded the Wazuh Manager stack to 4.14.5 |
| Kali agent connectivity | Corrected communication settings and verified TCP/1514 |
| Missing firewall telemetry | Enabled Windows Firewall logging and configured `localfile` ingestion |
| XML configuration errors | Corrected the configuration syntax and restarted the agent |

## Configuration Examples

Sanitized examples are available under [`configuration/`](configuration/README.md):

- Windows agent configuration example
- Windows Firewall `localfile` ingestion example

No credentials, tokens, API keys, or production secrets are included.

## Project Scope and Limitations

This project demonstrates **detection, investigation, correlation, and threat hunting**.

The following were **not implemented** and are documented as future work:

- Sysmon integration
- VirusTotal or other threat-intelligence enrichment
- Custom Wazuh detection rules
- Automated containment or response

See [`future-work/`](future-work/README.md) for the planned roadmap.

The `rules/` directory documents that this completed lab relied on Wazuh's existing detection and correlation capabilities; custom rules were not claimed as implemented.

## Repository Structure

```text
.
├── architecture/
│   ├── README.md
│   └── wazuh-soc-architecture.svg
├── comparison/
├── configuration/
│   ├── README.md
│   ├── firewall-localfile.xml
│   └── windows-agent-ossec.conf.example
├── detections/
│   ├── authentication-monitoring.md
│   ├── file-integrity-monitoring.md
│   └── network-scan-detection.md
├── future-work/
│   └── README.md
├── reports/
│   ├── WAZUH-Project-Report.md
│   └── WAZUH_Project_Report.pdf
├── rules/
├── screenshots/
│   └── README.md
└── README.md
```

## Security Note

No passwords, API keys, tokens, private keys, or production secrets should be committed to this repository. Configuration examples use placeholders where appropriate.

## Author

**Ibrahim Khaleel M**

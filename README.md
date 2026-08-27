# Wazuh-Based Security Monitoring and Threat Hunting SOC Lab

A hands-on Security Operations Center (SOC) laboratory built with Wazuh, Ubuntu Server, Windows 11, and Kali Linux. The project demonstrates centralized endpoint monitoring, File Integrity Monitoring (FIM), Windows Firewall telemetry analysis, reconnaissance detection, event correlation, and MITRE ATT&CK-based threat hunting.

> All testing was performed in a controlled private lab against systems used for authorized testing.

## Lab Environment

| System | Role | IP Address |
|---|---|---:|
| Ubuntu Server | Wazuh Manager / SIEM | `192.168.1.74` |
| Windows 11 | Monitored endpoint / Wazuh Agent | `192.168.1.70` |
| Kali Linux | Attack simulation + Wazuh Agent | `192.168.1.71` |

## Architecture

### Attack flow

```text
Kali Linux
     │ Nmap / controlled traffic
     ▼
Windows 11 Endpoint
```

### Security telemetry flow

```text
Windows 11 Agent ─────┐
                      ├──> Wazuh Manager ──> Wazuh Indexer ──> Dashboard
Kali Linux Agent ─────┘                                      Threat Hunting
```

## What Was Implemented

- Wazuh Manager, Dashboard, and Indexer deployment
- Windows 11 and Kali Linux agent integration
- File Integrity Monitoring for a controlled Windows directory
- Registry integrity monitoring
- Windows Security Event monitoring
- Windows Firewall telemetry ingestion
- Controlled Nmap SYN-scan simulation from Kali Linux
- Wazuh correlation validation for repeated firewall drop events
- MITRE ATT&CK-based threat hunting and event classification

## Detection Use Cases

### File and Registry Integrity Monitoring

Monitored `C:\Users\Public` and observed integrity-related alerts, including:

- Rule **550** — Integrity checksum changed
- Rule **594** — Registry key integrity checksum changed
- Rule **750** — Registry value integrity checksum changed

Integrity alerts were treated as investigation leads and validated with endpoint context rather than automatically classified as malicious.

### Windows Account and Authentication Monitoring

- Windows Event ID **4720** for local account creation
- Windows Event ID **4625** for failed logon activity
- Observed Wazuh Rule **60122** for logon-failure activity

### Network Scan Detection

A controlled SYN scan was performed from Kali Linux against the Windows endpoint:

```bash
nmap -sS 192.168.1.70
```

Windows Firewall telemetry was ingested from:

```text
C:\Windows\System32\LogFiles\Firewall\pfirewall.log
```

Wazuh correlated repeated firewall-drop events and generated:

| Rule ID | Description | Level |
|---|---|---:|
| **4151** | Multiple Firewall drop events from same source | **10** |

This demonstrates correlation: multiple underlying events can contribute to one higher-level alert instead of generating a separate alert for every packet.

## MITRE ATT&CK Threat Hunting

The reconnaissance scenario was classified as:

**T1046 — Network Service Discovery**

The Wazuh Threat Hunting interface was used to review alerts, event context, severity, related activity, and ATT&CK classifications.

## Troubleshooting Demonstrated

| Challenge | Resolution |
|---|---|
| Agent version mismatch | Upgraded the Wazuh Manager stack to 4.14.5 |
| Kali agent connectivity | Corrected communication settings and verified TCP/1514 |
| Missing firewall telemetry | Enabled Windows Firewall logging and configured `localfile` ingestion |
| XML configuration errors | Corrected the configuration syntax and restarted the agent |

## Project Scope and Limitations

This project demonstrates **detection, investigation, correlation and threat hunting**.

The following were **not implemented** and are planned as future enhancements:

- Sysmon integration
- VirusTotal enrichment
- Custom Wazuh detection rules
- Automated containment or response

Production deployments would also require additional rule tuning and contextual analysis to reduce false positives and alert fatigue.

## Repository Structure

```text
.
├── architecture/
├── comparison/
├── configuration/
├── detections/
├── reports/
│   └── WAZUH-Project-Report.md
├── rules/
└── README.md
```

## Security Note

No passwords, API keys, tokens, private keys, or production secrets should be committed to this repository. Any examples involving credentials should use placeholders only.

## Author

**Ibrahim Khaleel M**

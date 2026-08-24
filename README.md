# Wazuh SIEM Security Monitoring Lab

A hands-on SOC laboratory built around Wazuh for endpoint security monitoring, File Integrity Monitoring (FIM), authentication monitoring, custom detection rules, threat-intelligence enrichment, and MITRE ATT&CK-based investigation.

## Lab Overview

The documented environment uses:

- Wazuh Manager
- Wazuh Dashboard
- Wazuh Agent
- Windows endpoint
- Kali Linux
- VirusTotal threat-intelligence integration

The project focuses on collecting endpoint telemetry, detecting security-relevant events, validating alerts, and investigating activity from a SOC analyst perspective.

## Architecture

```text
Kali Linux / Attack Simulation
            |
            v
      Windows Endpoint
       + Wazuh Agent
            |
            v
       Wazuh Manager
            |
      +-----+-----+
      |           |
 Wazuh Dashboard  Threat Intelligence
                    |
                 VirusTotal
```

See [`architecture/`](architecture/) for the documented lab architecture.

## Core Use Cases

| Use Case | Security Goal |
|---|---|
| File Integrity Monitoring | Detect unauthorized file changes |
| Authentication Monitoring | Identify suspicious login activity |
| Custom Wazuh Rules | Convert security events into actionable alerts |
| Threat Intelligence | Enrich suspicious file indicators with VirusTotal |
| MITRE ATT&CK Mapping | Classify observed activity for investigation |

## Detection Engineering Workflow

```text
Endpoint / Attack Activity
          ↓
      Wazuh Agent
          ↓
      Wazuh Manager
          ↓
 Rule / Decoder / FIM Event
          ↓
       Alerting
          ↓
 Threat Intelligence Enrichment
          ↓
 Analyst Investigation
          ↓
 MITRE ATT&CK Mapping
```

## Project Evidence

The repository separates detection logic, configuration examples, screenshots, and the complete project report so the implementation can be reviewed without relying only on the PDF.

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

## Security Notes

- Configuration examples must contain placeholders rather than real API keys, passwords, or tokens.
- The laboratory was designed for controlled security testing and learning.
- Attack simulations should only be performed against systems you own or are explicitly authorized to test.

## Project Report

The complete project report is available under [`reports/`](reports/).

## Author

**Ibrahim Khaleel**
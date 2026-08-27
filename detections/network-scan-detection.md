# Network Scan Detection

## Objective

Detect repeated blocked connection attempts generated during controlled reconnaissance activity.

## Lab Scenario

A SYN scan was performed from the Kali Linux lab system against the Windows 11 endpoint:

```bash
nmap -sS 192.168.1.70
```

The Windows Firewall log was collected from:

```text
C:\Windows\System32\LogFiles\Firewall\pfirewall.log
```

## Detection Result

Individual firewall events were ingested by Wazuh. Repeated firewall-drop events from the same source were correlated into the higher-level alert:

| Rule ID | Description | Level |
|---|---|---:|
| 4151 | Multiple Firewall drop events from same source | 10 |

A single correlated alert can represent multiple underlying firewall events. Therefore, running multiple scans does not necessarily produce one high-level alert for every scan.

## Investigation Workflow

1. Identify the source IP and monitored endpoint.
2. Review the underlying firewall events around the alert timestamp.
3. Confirm whether the activity matches authorized lab testing.
4. Review related events for additional reconnaissance or authentication activity.
5. Record the finding and determine whether further containment is required.

## MITRE ATT&CK Context

The reconnaissance scenario was reviewed in the context of **T1046 — Network Service Discovery**.

## Evidence

See `screenshots/03-network-scan-detection.png` when the evidence image is added to the repository.

## Security Value

This use case demonstrates log ingestion, event correlation, alert triage, and the difference between raw telemetry and a higher-level security alert.

# Wazuh-Based Security Monitoring and Threat Hunting SOC Lab

**Prepared by:** Ibrahim Khaleel M  
**Year:** 2026

## 1. Introduction

This project focused on building a controlled Security Operations Center (SOC) lab using Wazuh for centralized log monitoring, endpoint visibility, alert correlation, and threat hunting. The environment included a Wazuh Manager on Ubuntu Server, a Windows 11 endpoint, and a Kali Linux system used for controlled attack simulation and monitored Linux telemetry.

The lab validated the following controlled security-monitoring scenarios:

- File Integrity Monitoring (FIM)
- Registry integrity monitoring
- User account activity monitoring
- Firewall-based scan detection
- Threat hunting using MITRE ATT&CK mapping
- Multi-endpoint log collection and analysis

All testing was performed in a controlled private lab using systems owned or administered for the exercise. No production or third-party systems were targeted.

## 2. Lab Environment and Network Configuration

| System | Role | IP Address | Wazuh Version |
|---|---|---:|---:|
| Ubuntu Server | Wazuh Manager / SIEM | `192.168.1.74` | `4.14.5` |
| Windows 11 | Monitored endpoint / Wazuh Agent | `192.168.1.70` | `4.7.5` |
| Kali Linux | Attacker + Wazuh Agent | `192.168.1.71` | `4.14.5` |

The final Wazuh server stack was upgraded to version 4.14.5. The Windows agent remained on 4.7.5 and continued to communicate successfully with the manager.

## 3. Tools and Technologies

| Technology | Purpose |
|---|---|
| Wazuh | SIEM monitoring, analysis and alerting |
| Ubuntu Server | Wazuh Manager host |
| Windows 11 | Monitored endpoint |
| Kali Linux | Controlled attack simulation and Linux telemetry |
| Nmap | Network reconnaissance simulation |
| Windows Security Events | Authentication and account telemetry |
| Windows Defender Firewall | Network telemetry and dropped-packet logging |
| Wazuh Syscheck / FIM | File and registry integrity monitoring |
| Wazuh Indexer | Alert storage and search |
| Wazuh Dashboard | Investigation and threat hunting interface |
| MITRE ATT&CK | Threat classification and technique mapping |

## 4. Project Architecture

### Attack flow

```text
Kali Linux
(Attack Simulation)
       |
       | Nmap / controlled traffic
       v
Windows 11 Endpoint
(Wazuh Agent)
```

### Security telemetry flow

```text
Windows 11 Wazuh Agent ─────┐
                            │
Kali Linux Wazuh Agent ─────┼──> Wazuh Manager ──> Wazuh Indexer
                            │                          │
                            └──────────────────────────┘
                                                       |
                                                       v
                                             Wazuh Dashboard
                                             / Threat Hunting
```

Attack traffic was generated from Kali against the Windows endpoint. Endpoint and agent telemetry was forwarded to the Wazuh Manager for processing and correlation, indexed for search, and reviewed through the Dashboard and Threat Hunting views.

## 5. Detection Methodology

The general detection workflow was:

```text
Attack / Endpoint Activity
          ↓
      Wazuh Agent
          ↓
      Wazuh Manager
          ↓
 Rule Processing / Correlation
          ↓
      Wazuh Indexer
          ↓
 Dashboard / Threat Hunting
          ↓
 Investigation and MITRE Classification
          ↓
 Response Recommendation
```

The analyst reviewed event context, rule severity, source information, related events, and MITRE ATT&CK mapping before classifying activity.

## 6. Wazuh Deployment

Installed and configured:

- Wazuh Manager
- Wazuh Dashboard
- Wazuh Indexer

The final server stack version was 4.14.5. Service operation, dashboard accessibility, and endpoint connectivity were verified.

## 7. Endpoint Integration

### Windows 11 Agent

The Wazuh Agent was installed on the Windows endpoint and configured with the Wazuh Manager address, agent communication settings, and log forwarding. The agent was verified as active and Windows logs were successfully ingested.

### Kali Linux Agent

The Wazuh Agent was installed on Kali Linux so the system could provide Linux endpoint telemetry while also serving as the controlled attack-simulation host.

The Kali agent initially attempted to connect to the Wazuh Manager on TCP/5601 instead of the agent communication port. The configuration was corrected to use TCP/1514. The Kali agent also initially failed because its 4.14.5 version was newer than the original manager version. The Wazuh Manager stack was upgraded to 4.14.5, after which the agent connected successfully.

Agent communication was validated on TCP/1514.

## 8. Log Collection and Telemetry Sources

| Telemetry Source | Endpoint / Component | Purpose |
|---|---|---|
| Windows Security Events | Windows 11 | Authentication and account activity |
| File Integrity Monitoring (Syscheck) | Windows 11 | File and registry integrity changes |
| Windows Firewall Log | Windows 11 | Network connection and dropped-packet telemetry |
| Wazuh Manager / Indexer | Ubuntu Server | Rule processing, correlation, storage and search |

## 9. File Integrity Monitoring (FIM)

### Objective

Monitor selected Windows directories and registry activity for integrity changes that may require investigation.

### Configuration

The Windows agent was configured to monitor `C:\Users\Public` and the scan interval was reduced to 60 seconds for lab testing:

```xml
<directories check_all="yes">C:\Users\Public</directories>
<frequency>60</frequency>
```

The agent was restarted after configuration changes.

### Controlled Test

A file was created and modified inside the monitored directory:

```cmd
echo hacked > C:\Users\Public\test.txt
echo modified >> C:\Users\Public\test.txt
```

### Detection Results

Wazuh generated integrity-related events during the lab, including:

- Rule 550 — Integrity checksum changed
- Rule 594 — Registry key integrity checksum changed
- Rule 750 — Registry value integrity checksum changed

Rules 594 and 750 were observed at Wazuh level 5, while Rule 550 was observed at level 7.

### Detection Interpretation

The alerts were treated as investigation leads rather than automatic proof of compromise. Windows and installed applications can legitimately modify registry data, so registry-integrity alerts require context and validation before being classified as persistence or malicious activity.

### Analyst Action

For an integrity alert, an analyst should validate the affected path or registry item, identify the process or user responsible where possible, check timestamps and related events, and determine whether the change was authorized.

## 10. Windows Firewall Telemetry Integration

### Objective

Integrate Windows Firewall logs into Wazuh to provide network telemetry and support detection of suspicious activity.

Windows Firewall logging was enabled for dropped packets and successful connections.

Firewall log location:

```text
C:\Windows\System32\LogFiles\Firewall\pfirewall.log
```

The Wazuh Windows agent was configured to ingest the file using:

```xml
<localfile>
  <location>C:\Windows\System32\LogFiles\Firewall\pfirewall.log</location>
  <log_format>syslog</log_format>
</localfile>
```

The Wazuh Agent service was restarted after the configuration change.

## 11. Network Scan Detection

### Objective

Detect reconnaissance activity generated from the Kali Linux attack-simulation host against the Windows endpoint.

### Attack Simulation

A controlled Nmap SYN scan was performed against the Windows endpoint:

```bash
nmap -sS 192.168.1.70
```

### Detection Result

Wazuh correlated repeated firewall-drop events and generated a higher-level detection:

| Rule ID | Description | Observed Level |
|---|---|---:|
| 4151 | Multiple Firewall drop events from same source | 10 |

This demonstrates how the firewall log acted as the telemetry source while Wazuh provided the correlation and alerting layer.

### Correlation Behavior

Multiple firewall-drop events from the same source were correlated into a higher-level Rule 4151 alert rather than producing a separate alert for every packet. This reduces repetitive alert volume. The underlying firewall events can still be reviewed to validate source IP, destination, protocol, ports and timestamps.

### MITRE ATT&CK Mapping

The reconnaissance behavior was mapped to:

**T1046 — Network Service Discovery**

## 12. Windows Account and Authentication Monitoring

### Objective

Detect local account creation and repeated authentication failures using Wazuh-collected Windows Security events.

### Account Creation Test

A temporary lab account was created using a lab-only password:

```cmd
net user attacker <LAB_TEST_PASSWORD> /add
```

Windows Event ID **4720** was generated for the account-creation activity. Wazuh displayed account-related activity with an observed MITRE ATT&CK mapping to **T1098 — Account Manipulation**. The Windows event was treated as the primary evidence, with the MITRE mapping used as supporting context.

### Authentication Failure Test

Repeated incorrect Windows login attempts generated Windows Security Event ID **4625**. Wazuh displayed the resulting logon-failure activity, including Rule **60122** in the observed dashboard view.

### Analyst Interpretation

A single failed-login event is not proof of a brute-force attack. An analyst should examine frequency, source context, affected account, timing, and related authentication events to determine whether the activity is benign or suspicious.

### Response Considerations

For a production alert, an analyst would validate the account and source context before disabling an unauthorized account, forcing a credential reset, blocking a source where appropriate, or escalating the incident. No automated containment was executed in this lab.

## 13. Alert Investigation Workflow

The Nmap detection was investigated using the following workflow:

| Stage | Activity | Outcome |
|---|---|---|
| 1. Attack | Kali performed an Nmap SYN scan against Windows | Reconnaissance traffic generated |
| 2. Telemetry | Windows Firewall recorded dropped packets | Firewall telemetry available to Wazuh |
| 3. Detection | Wazuh correlated repeated firewall drops | Rule 4151 generated |
| 4. Investigation | Reviewed source endpoint, timestamp and event context | Activity identified as network scanning |
| 5. Mapping | Compared behavior with MITRE ATT&CK | T1046 classified |
| 6. Response | Considered source review/blocking and service exposure reduction | Response guidance documented |

## 14. Detection and Investigation Matrix

| Use Case | Primary Telemetry | Observed Evidence | MITRE / Rule | Analyst Action |
|---|---|---|---|---|
| FIM / Registry | Wazuh Syscheck | Rules 550, 594, 750 | Integrity events | Validate affected path and context |
| Nmap Scan | Windows Firewall | Rule 4151, Level 10 | T1046 / Network Service Discovery | Investigate source, ports and exposure |
| Account Creation | Windows Security Log | Event 4720 | T1098 / Account Manipulation | Validate user, initiator and authorization |
| Failed Logon | Windows Security Log | Event 4625; Rule 60122 | Investigation lead | Check source, frequency and account status |

## 15. Threat Hunting and MITRE ATT&CK Analysis

The Wazuh Threat Hunting module was used to:

- investigate alerts
- analyze correlated events
- identify attack patterns
- map detections to MITRE ATT&CK techniques

Observed event classifications included Persistence, Defense Evasion, Privilege Escalation and Impact. These classifications were treated as contextual evidence and not as automatic proof that a malicious technique had been successfully executed.

## 16. Troubleshooting Experience

| Challenge | Resolution |
|---|---|
| Agent version mismatch | Upgraded the Wazuh Manager stack to 4.14.5 and revalidated compatibility |
| Kali agent connection issues | Corrected manager communication settings, authenticated the agent and verified TCP/1514 connectivity |
| Missing firewall telemetry | Enabled Windows Firewall logging and onboarded `pfirewall.log` through Wazuh `localfile` configuration |
| XML configuration errors | Corrected the `localfile` XML syntax and restarted the Wazuh Agent |

## 17. Implementation Status

| Capability | Status | Evidence / Notes |
|---|---|---|
| Multi-endpoint Wazuh lab | Completed | Ubuntu Manager, Windows 11 agent and Kali agent active |
| Centralized log monitoring | Completed | Windows Security, FIM and Firewall telemetry |
| File Integrity Monitoring | Completed | Syscheck configuration and integrity alerts |
| Firewall telemetry / scan detection | Completed | `pfirewall.log` + Rule 4151 during Nmap scan |
| Threat hunting / MITRE mapping | Completed | Wazuh Threat Hunting views and ATT&CK mapping |
| VirusTotal enrichment | Not implemented | Future enhancement |
| Sysmon endpoint telemetry | Not implemented | Future enhancement |
| Automated containment | Not implemented | No automated response executed in the lab |

## 18. Limitations

This lab focused on detection, investigation and threat classification. Automated containment, VirusTotal enrichment and Sysmon-based endpoint telemetry were not implemented and are intentionally excluded from the completed-project claims.

The FIM scan interval was reduced to 60 seconds for laboratory testing; production deployments should tune scan frequency according to workload and monitoring requirements.

Default endpoint telemetry can contain benign or repetitive events. Production deployments require contextual analysis and, where appropriate, rule tuning to reduce false positives and alert fatigue.

## 19. Key Achievements

- Built a functional multi-endpoint SOC lab using Wazuh
- Integrated Windows and Kali Linux endpoints
- Configured centralized security telemetry
- Implemented File Integrity Monitoring
- Integrated Windows Firewall telemetry
- Detected controlled reconnaissance activity using Wazuh correlation
- Investigated alerts using source, timestamp, severity and event context
- Performed MITRE ATT&CK-based threat hunting
- Documented practical troubleshooting and detection-validation workflows

## 20. Conclusion

The project demonstrated a practical Wazuh-based SOC workflow for endpoint monitoring, centralized telemetry collection, alert correlation, investigation and threat hunting. The controlled lab validated detection of file and registry integrity changes, Windows account and authentication events, firewall activity and reconnaissance generated from Kali Linux.

The project provided hands-on exposure to SIEM operations, endpoint monitoring, log analysis, detection validation, alert correlation, threat hunting and MITRE ATT&CK-based classification.

## 21. Future Enhancements

- Sysmon integration for richer Windows process and command-line telemetry
- VirusTotal enrichment for file-hash based threat intelligence
- Custom Wazuh detection rules
- Additional PowerShell and persistence detection use cases
- Automated response and containment workflows

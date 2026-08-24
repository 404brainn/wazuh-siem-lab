# Threat-Intelligence Driven SOC Lab Using Wazuh SIEM

**Prepared by:** Ibrahim Khaleel M  
**Year:** 2026

## 1. Introduction

This project focused on building a functional Security Operations Center (SOC) lab using Wazuh for centralized log monitoring, threat detection, endpoint visibility, and threat hunting.

The environment included a Wazuh Manager deployed on Ubuntu, a Windows 11 endpoint, and a Kali Linux attacker machine.

Documented scenarios include:

- File Integrity Monitoring (FIM)
- Unauthorized registry modifications
- User account activity monitoring
- Firewall-based scan detection
- Threat hunting using MITRE ATT&CK mapping
- Multi-endpoint log collection and analysis

## 2. Lab Environment

| System | Purpose |
|---|---|
| Ubuntu Server | Wazuh Manager / SIEM |
| Windows 11 | Monitored endpoint |
| Kali Linux | Attacker machine |

## 3. Architecture

```text
Kali Linux (Attacker)
        ↓
Windows 11 Endpoint (Wazuh Agent)
        ↓
Ubuntu Wazuh Manager
        ↓
Wazuh Dashboard / Threat Hunting
```

## 4. Tools and Technologies

- Wazuh
- Ubuntu Server
- Windows 11
- Kali Linux
- Nmap
- Windows Defender Firewall
- Wazuh Agents
- File Integrity Monitoring (FIM)
- MITRE ATT&CK Framework

## 5. Wazuh Deployment

Installed and configured:

- Wazuh Manager
- Wazuh Dashboard
- Wazuh Indexer

Verified service operation, dashboard accessibility, and agent connectivity.

## 6. Endpoint Integration

### Windows 11

Installed the Wazuh Agent and configured the Manager IP address, agent communication, and log forwarding. The Windows agent was verified as active and Windows logs were successfully ingested.

### Kali Linux

Installed and configured the Wazuh Agent. The lab encountered version mismatch, authentication, and Manager connectivity issues. These were resolved and the Kali endpoint became active in the Wazuh dashboard.

## 7. File Integrity Monitoring

### Objective

Monitor critical directories and registry activity for unauthorized changes.

### Configuration

The Windows agent monitored:

```xml
<directories check_all="yes">C:\Users\Public</directories>
<frequency>60</frequency>
```

### Simulation

Controlled file changes were generated in the monitored directory:

```cmd
echo hacked > C:\Users\Public\test.txt
echo modified >> C:\Users\Public\test.txt
```

### Detection Results

Wazuh generated integrity and registry alerts, including:

- Integrity checksum changed
- Registry key integrity checksum changed
- Registry value integrity checksum changed

Documented rule IDs include **594**, **750**, and **550**.

### Impact

The scenario demonstrated detection of unauthorized file modifications, persistence-related registry changes, and suspicious endpoint activity.

## 8. Windows Firewall Telemetry

### Objective

Integrate Windows Firewall logs into Wazuh to detect suspicious network activity.

The project enabled dropped-packet and successful-connection logging.

Firewall log location:

```text
C:\Windows\System32\LogFiles\Firewall\pfirewall.log
```

The Wazuh agent was configured to ingest the firewall log using a `localfile` configuration with `syslog` format.

## 9. Network Scan Detection

### Objective

Detect reconnaissance activity originating from the Kali Linux attacker system.

### Attack Simulation

The report documents an Nmap SYN scan against the Windows endpoint:

```bash
nmap -sS 192.168.1.70
```

### Detection Result

Wazuh generated a correlated firewall alert using **Rule ID 4151**, described as multiple firewall drop events from the same source.

This demonstrated firewall telemetry analysis, SIEM event correlation, and reconnaissance detection.

## 10. MITRE ATT&CK Mapping

The reconnaissance scenario was mapped to:

**T1046 — Network Service Scanning**

The Wazuh Threat Hunting module was used to investigate alerts, analyze correlated events, identify attack patterns, and map detections to MITRE ATT&CK techniques.

The report also documents observed tactics including:

- Persistence
- Defense Evasion
- Privilege Escalation
- Impact

## 11. Key Achievements

- Built a functional multi-endpoint SOC lab
- Integrated Windows and Kali Linux endpoints
- Configured centralized log monitoring
- Implemented File Integrity Monitoring
- Integrated Windows Firewall telemetry
- Detected reconnaissance activity using Wazuh correlation rules
- Performed threat hunting using MITRE ATT&CK
- Simulated attack scenarios safely in a lab environment

## 12. Troubleshooting Experience

| Challenge | Resolution |
|---|---|
| Agent version mismatch | Upgraded Wazuh Manager |
| Kali agent connection issues | Corrected Manager configuration |
| Missing firewall telemetry | Enabled Windows Firewall logging |
| XML configuration errors | Corrected `localfile` configuration syntax |

## 13. Conclusion

The project demonstrated Wazuh as a centralized SIEM platform for monitoring endpoint activity, detecting suspicious behavior, and performing threat hunting across multiple systems.

The lab validated detection of:

- File modifications
- Registry changes
- Firewall events
- Reconnaissance activity

It provided practical exposure to SIEM operations, endpoint monitoring, detection engineering, log analysis, and SOC workflows.

## 14. Future Enhancements

The source report lists the following as future enhancements rather than completed features:

- Sysmon integration
- VirusTotal integration
- Custom detection rules
- Malware behavior analysis
- PowerShell attack detection

## Evidence

See the `screenshots/` directory for selected evidence extracted from the original project report.
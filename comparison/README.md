# Wazuh vs Splunk — Portfolio Comparison

These two projects demonstrate different SOC workflows rather than duplicating the same detection scenario.

| Area | Wazuh Lab | Splunk Lab |
|---|---|---|
| Primary focus | Endpoint monitoring and threat hunting | SPL-based detection engineering |
| Endpoint agents | Wazuh Agent on Windows and Kali | Splunk Universal Forwarder / collected telemetry |
| FIM | Demonstrated | Not the primary use case |
| Firewall telemetry | Demonstrated | Not the primary use case |
| Reconnaissance detection | Nmap SYN scan + Wazuh correlation | Separate Splunk TCP port-scan detection |
| Query / detection workflow | Wazuh rules, event filters, Threat Hunting | SPL searches, reports, alerts |
| MITRE ATT&CK | T1046 documented for reconnaissance | ATT&CK mapping used in the Splunk project |
| Dashboards | Wazuh Dashboard / Threat Hunting | Splunk dashboards |

## Interview Takeaway

The strongest comparison is not that one SIEM is "better." The portfolio shows that the analyst can work with two different SIEM approaches:

- **Wazuh:** agent-based endpoint telemetry, FIM, firewall log ingestion, correlation, and threat hunting.
- **Splunk:** structured event search using SPL, detection logic, alerting, and dashboard-driven investigation.

Avoid claiming measured differences in detection latency or query performance unless those metrics were actually tested and recorded.
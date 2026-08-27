# Evidence Screenshots

This directory contains screenshots captured from the completed Wazuh SOC lab. Each image supports a specific documented detection or monitoring result.

## Evidence Files

| File | Evidence |
|---|---|
| `01-agent-status.png` | Wazuh Endpoints view showing Kali and Windows agents active |
| `02-mitre-account-events.png` | MITRE ATT&CK view showing account-related activity, including T1098/T1484 context |
| `03-mitre-authentication-events.png` | MITRE ATT&CK view showing repeated logon-failure activity and Rule 60122 |
| `04-fim-alerts.png` | Wazuh Threat Hunting view showing integrity-related alerts (Rules 550, 594 and 750) |
| `05-firewall-rule-4151.png` | Firewall search showing Rule 4151, level 10 |
| `06-mitre-threat-hunting.png` | Wazuh Threat Hunting view used for MITRE ATT&CK investigation |

## Evidence Standard

Each screenshot should support a specific claim made in the project report or detection documentation. Avoid screenshots that expose passwords, API keys, tokens, private keys, or other secrets.

The repository documents completed functionality only. VirusTotal enrichment, Sysmon integration, custom detection rules, and automated containment are future enhancements and are not represented as completed evidence.

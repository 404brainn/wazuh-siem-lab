# Threat Intelligence Scope

## Report Status

The current Wazuh project report does **not** document a completed VirusTotal integration or malicious-file detection workflow. VirusTotal integration is listed under future enhancements.

Therefore, this repository does not claim that VirusTotal-based detection was implemented in the completed lab.

## Documented Detection Capabilities

The completed project demonstrates:

- File Integrity Monitoring (FIM)
- Registry integrity monitoring
- Windows Firewall telemetry
- Reconnaissance detection using Wazuh correlation
- MITRE ATT&CK-based threat hunting

## Future Enhancement

A future version can add threat-intelligence enrichment using VirusTotal, with API credentials stored securely outside the repository.

## Security Note

Never commit API keys, passwords, tokens, or other secrets to GitHub.
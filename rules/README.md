# Detection Rules Scope

This completed lab did **not** implement custom Wazuh detection rules.

The project validated Wazuh's existing detection and correlation capabilities using collected Windows Security Events, integrity-monitoring events, and Windows Firewall telemetry.

Examples observed during the lab include:

- Rule **550** — Integrity checksum changed
- Rule **594** — Registry key integrity checksum changed
- Rule **750** — Registry value integrity checksum changed
- Rule **4151** — Multiple Firewall drop events from same source
- Rule **60122** — Logon-failure activity observed during authentication monitoring

Custom rule development is documented as future work. If custom rules are added later, each rule should include its purpose, log source, detection logic, severity, expected alert, false-positive considerations, and justified MITRE ATT&CK mapping.

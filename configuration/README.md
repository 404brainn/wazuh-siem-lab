# Configuration Examples

This directory contains sanitized configuration examples based on the completed lab.

## Files

- `windows-agent-ossec.conf.example` — Windows Wazuh agent example showing manager connectivity, Security Event collection, FIM for `C:\Users\Public`, and firewall log ingestion.
- `firewall-localfile.xml` — Minimal `localfile` example for collecting `pfirewall.log`.

## Safe Publishing Rules

Never commit:

- API keys
- Passwords
- Authentication tokens
- Private keys
- Production secrets

Use placeholders such as:

```text
<WAZUH_MANAGER_IP>
<API_KEY>
<AGENT_NAME>
```

These examples are for documentation and lab reproduction. Validate configuration syntax and test in an isolated environment before use.

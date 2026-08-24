# Configuration

This directory is reserved for sanitized Wazuh configuration examples.

## Safe Publishing Rules

Never commit:

- API keys
- Passwords
- Authentication tokens
- Private keys
- Internal IPs or hostnames that should remain private

Use placeholders such as:

```text
<MANAGER_IP>
<VIRUSTOTAL_API_KEY>
<AGENT_NAME>
```

Configuration examples should be validated in the isolated lab before being used elsewhere.

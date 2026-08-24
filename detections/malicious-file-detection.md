# Malicious File Detection and Threat Intelligence

## Objective

Use Wazuh endpoint telemetry and VirusTotal enrichment to investigate suspicious file indicators in the controlled laboratory environment.

## Detection Flow

```text
Suspicious File
      ↓
 Wazuh Endpoint
      ↓
 Wazuh Alert / Indicator
      ↓
VirusTotal Enrichment
      ↓
 Analyst Investigation
```

## Analyst Workflow

1. Review the Wazuh alert.
2. Identify the affected endpoint and file indicator.
3. Review the available threat-intelligence result.
4. Determine whether the activity is expected or suspicious.
5. Document the finding and recommended response.

## Security Note

API credentials must never be committed to GitHub. Any configuration example in this repository uses placeholders or redacted values.

## Evidence

Add the corresponding threat-intelligence screenshot under `screenshots/`.

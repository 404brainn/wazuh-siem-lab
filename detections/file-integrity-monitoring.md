# File Integrity Monitoring (FIM)

## Objective

Detect unauthorized or unexpected changes to monitored files and directories using Wazuh File Integrity Monitoring.

## Detection Flow

```text
Monitored File
    ↓
Wazuh Agent
    ↓
FIM Event
    ↓
Wazuh Manager
    ↓
Alert
    ↓
Analyst Investigation
```

## Analyst Checks

- Which file changed?
- Which endpoint generated the event?
- What changed and when?
- Was the change expected?
- Is the affected file associated with suspicious activity?

## Evidence

Add the corresponding Wazuh FIM screenshot under `screenshots/`.

## Security Value

FIM provides visibility into file modifications that may indicate unauthorized configuration changes, tampering, persistence activity, or other endpoint compromise indicators.

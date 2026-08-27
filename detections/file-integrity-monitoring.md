# File Integrity Monitoring (FIM)

I used Wazuh FIM to watch for changes in selected files and directories.

## What Happens

```text
File change
    ↓
Wazuh Agent
    ↓
FIM event
    ↓
Wazuh Manager
    ↓
Alert
```

## What I Checked

When an integrity alert appeared, I checked:

- Which file or registry item changed
- Which endpoint reported it
- When the change happened
- Whether the change was expected
- Whether there were other related events

An integrity alert does not automatically mean the system was compromised. It is a starting point for investigation.

## Evidence

The related screenshot is available in the `screenshots` folder as `04-fim-alerts.png`.

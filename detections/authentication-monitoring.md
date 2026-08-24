# Authentication Monitoring

## Objective

Monitor authentication-related events collected by Wazuh and identify activity that requires analyst investigation.

## Detection Flow

```text
Authentication Event
        ↓
    Wazuh Agent
        ↓
   Wazuh Manager
        ↓
      Alert
        ↓
 Analyst Investigation
```

## Investigation Questions

- Which endpoint generated the event?
- Which account was involved?
- Was the activity successful or failed?
- Does the source or timing look unusual?
- Are there related events before or after the authentication event?

## Evidence

Add the corresponding authentication-monitoring screenshot under `screenshots/`.

## Security Value

Authentication monitoring provides a foundation for identifying suspicious account activity and correlating endpoint events during incident investigation.

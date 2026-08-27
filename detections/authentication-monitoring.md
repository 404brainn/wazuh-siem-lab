# Authentication Monitoring

This part of the lab focused on Windows account and logon events collected by Wazuh.

## Event Flow

```text
Windows event
    ↓
Wazuh Agent
    ↓
Wazuh Manager
    ↓
Alert
    ↓
Investigation
```

## What I Looked At

For an authentication-related alert, I checked:

- The endpoint that generated the event
- The account involved
- Whether the logon succeeded or failed
- The source and time of the activity
- Other events around the same time

I used the events as investigation data rather than treating every failed logon as an attack.

## Evidence

The screenshots `02-mitre-account-events.png` and `03-mitre-authentication-events.png` show the account and authentication-related events reviewed in Wazuh.

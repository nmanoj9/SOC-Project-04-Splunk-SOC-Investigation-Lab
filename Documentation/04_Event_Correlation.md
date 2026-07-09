# Event Correlation

## Objective

The objective of this investigation was to correlate Windows Security Logs and Sysmon telemetry to reconstruct the sequence of events observed during the lab.

Rather than analyzing individual events in isolation, multiple log sources were correlated to provide a complete view of user authentication and endpoint activity.

---

## Correlated Events

| Time | Event ID | Description |
|------|----------|-------------|
| 10:56:15 AM | 4625 | Failed Logon |
| 10:56:38 AM | 4624 | Successful Logon |
| 10:56:38 AM | 4672 | Special Privileges Assigned |
| 10:57:24 AM | Sysmon Event ID 1 | Process Creation (Notepad.exe) |
| 10:57:52 AM | Sysmon Event ID 3 | Network Connection (chrome.exe) |
| 10:58:00 AM | Sysmon Event ID 5 | Process Termination (Notepad.exe) |

---

# Investigation Timeline

```
Failed Logon (4625)
        │
        ▼
Successful Logon (4624)
        │
        ▼
Special Privileges Assigned (4672)
        │
        ▼
Process Creation (Sysmon Event ID 1)
        │
        ▼
Network Connection (Sysmon Event ID 3)
        │
        ▼
Process Termination (Sysmon Event ID 5)
```

---

# Correlation Analysis

## Authentication Phase

The investigation began with multiple failed interactive logon attempts (Event ID 4625).

A successful authentication (Event ID 4624) was observed shortly afterward, indicating that the user successfully authenticated following the failed attempts.

Immediately after authentication, Windows generated Event ID 4672, confirming that administrative privileges were assigned to the newly created logon session.

---

## Endpoint Activity

Following successful authentication, Sysmon telemetry was analyzed to observe endpoint activity.

The investigation confirmed:

- Notepad.exe was launched by explorer.exe.
- Google Chrome generated normal mDNS (UDP/5353) network communication.
- Notepad.exe terminated normally after execution.

These events demonstrated normal user activity following authentication.

---

# Investigation Outcome

Correlation of Windows Security Logs and Sysmon telemetry confirmed the following sequence:

1. Multiple failed authentication attempts occurred.
2. Authentication eventually succeeded.
3. Administrative privileges were assigned.
4. User processes were executed.
5. Normal network communication occurred.
6. Processes terminated normally.

The investigation reconstructed the complete activity timeline using multiple log sources.

---

# Analyst Assessment

The observed activity resembled a brute-force authentication scenario due to repeated failed logons.

However, correlation across Windows Security Logs and Sysmon telemetry confirmed that all events were intentionally generated within a controlled lab environment.

No indicators of compromise, persistence mechanisms, privilege abuse, or malicious process execution were identified.

---

## Evidence Collected

- Authentication Timeline
- Windows Security Event Correlation
- Sysmon Event Correlation
- Process Lifecycle Analysis
- Investigation Timeline

---

## Conclusion

Event correlation significantly improves investigation accuracy by combining authentication logs with endpoint telemetry.

This investigation demonstrates the value of correlating Windows Security Logs with Sysmon telemetry to reconstruct authentication events, endpoint activity, and complete process lifecycles. Cross-source correlation significantly improves investigation accuracy and analyst confidence.

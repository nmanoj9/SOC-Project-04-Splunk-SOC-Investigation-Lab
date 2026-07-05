# Windows Security Log Analysis

## Objective

The objective of this investigation was to validate Windows Security Log ingestion into Splunk Enterprise and analyze authentication-related events to identify failed logon attempts, successful authentication, and privileged logon activity.

---

## Event IDs Investigated

| Event ID | Description  |
|----------|--------------|
| 4625 | Failed Logon     |
| 4624 | Successful Logon |
| 4672 | Special Privileges Assigned to New Logon |

---

# Event ID 4625 – Failed Logon

## Investigation

Windows Security Event ID 4625 records failed authentication attempts.

During this investigation, multiple failed interactive logons were intentionally generated from the local host to simulate repeated authentication failures.

The collected event contained:

- Account Name
- Source Network Address
- Logon Type
- Failure Reason
- Status
- Sub Status
- Timestamp

The analysis confirmed four consecutive failed interactive logon attempts originating from the same source address (127.0.0.1).

---

## Analyst Findings

- Four failed logon attempts observed.
- Logon Type: 2 (Interactive)
- Source Address: 127.0.0.1
- Failure occurred within a short time window.
- Activity was intentionally generated during the lab.

---

# Event ID 4624 – Successful Logon

## Investigation

Following the failed authentication attempts, Windows Security Event ID 4624 was analyzed to verify whether authentication eventually succeeded.

The investigation confirmed a successful logon after the failed attempts.

Observed fields included:

- Account Name
- Logon Type
- Process Name
- Authentication Package
- Source Network Address

---

## Analyst Findings

- Authentication completed successfully.
- No suspicious authentication source identified.
- Authentication originated from the local system.
- Process information matched expected Windows behavior.

---

# Event ID 4672 – Special Privileges Assigned

## Investigation

Event ID 4672 records administrative privileges assigned to a newly authenticated session.

The investigation verified that Windows assigned elevated privileges immediately after successful authentication.

Observed privileges included:

- SeDebugPrivilege
- SeBackupPrivilege
- SeRestorePrivilege
- SeTakeOwnershipPrivilege
- SeImpersonatePrivilege

---

## Analyst Findings

- Privileged logon occurred immediately after authentication.
- Assigned privileges matched expected Windows administrative behavior.
- No abnormal privilege escalation indicators were identified.

---

# Investigation Summary

The Windows Security Log investigation successfully validated authentication events generated during the lab.

The collected evidence demonstrated the complete authentication lifecycle:

Failed Logon (4625)

↓

Successful Logon (4624)

↓

Special Privileges Assigned (4672)

No malicious behavior was identified, and all observed activity matched the controlled lab scenario.

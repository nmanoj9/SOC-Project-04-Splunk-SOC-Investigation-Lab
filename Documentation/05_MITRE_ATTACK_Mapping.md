# MITRE ATT&CK Mapping

## Objective

The objective of this section is to map the observed activities to the MITRE ATT&CK framework where appropriate. Only activities supported by the collected evidence are mapped to ensure accurate representation of the investigation.

---

## MITRE ATT&CK Mapping

| Technique ID | Technique | Evidence |
|--------------|-----------|----------|
| T1110 | Brute Force | Multiple failed authentication attempts (Event ID 4625) |
| T1078 | Valid Accounts | Successful authentication following repeated failed logon attempts (Event ID 4624) |

---

## Technique: T1110 – Brute Force

### Evidence

- Windows Security Event ID 4625
- Four consecutive failed authentication attempts
- Authentication attempts occurred within a short time window
- Detection rule successfully identified the activity

### Analyst Assessment

Repeated authentication failures against the same account are consistent with brute-force password guessing techniques. In this lab, the activity was intentionally generated to validate the detection logic and demonstrate investigation techniques.

---

## Technique: T1078 – Valid Accounts

### Evidence

- Windows Security Event ID 4624
- Successful authentication after repeated failed logon attempts

### Analyst Assessment

Following the failed authentication attempts, a successful logon was recorded using a valid account. During this investigation, the activity represented expected behavior within the controlled lab environment.

---

## Activities Not Mapped

The following events were intentionally **not** mapped to MITRE ATT&CK:

- Windows Security Event ID 4672
- Sysmon Event ID 1 (Process Creation)
- Sysmon Event ID 3 (Network Connection)
- Sysmon Event ID 5 (Process Termination)

### Reason

These events represented normal Windows operating system behavior generated during the lab. Mapping routine administrative activity or expected application behavior to adversary techniques without supporting evidence would be inaccurate and could lead to misleading investigation results.

---

## Investigation Summary

Only authentication-related activity demonstrated behavior consistent with MITRE ATT&CK techniques.

The remaining Windows Security and Sysmon events were analyzed as part of the investigation but were intentionally excluded from ATT&CK mapping because they did not indicate malicious activity.

---

## Conclusion

Accurate MITRE ATT&CK mapping requires both technical evidence and investigative context. This investigation demonstrates the importance of mapping only confirmed adversary behaviors while avoiding unnecessary mappings for normal system activity.

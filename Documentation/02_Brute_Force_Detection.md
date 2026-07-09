# Brute Force Detection

## Objective

The objective of this investigation was to detect repeated authentication failures that may indicate a brute-force attack against a Windows account.

Windows Security Event ID 4625 was analyzed using Splunk Search Processing Language (SPL) to identify multiple failed logon attempts occurring within a short period of time.

---

## Detection Logic

The detection rule searched for:

- Windows Security Event ID 4625
- Failed interactive logons
- Same account
- Same source IP address
- Four or more failed attempts within a five-minute time window

---

## SPL Query

```spl
index=main EventCode=4625 earliest="06/22/2026:10:56:00" latest="06/22/2026:10:57:00"
| eval Target_Account=mvindex(Account_Name,0)
| bin _time span=5m
| stats count as Failed_Attempts earliest(_time) as First_Attempt latest(_time) as Last_Attempt by Target_Account Source_Network_Address Logon_Type
| where Failed_Attempts>=4
| convert ctime(First_Attempt) ctime(Last_Attempt)
```


## SPL Query Explanation

| Command | Purpose |
|---------|---------|
| eval | Extracts the target account from the multivalue field |
| bin | Groups events into five-minute intervals |
| stats | Counts failed authentication attempts |
| where | Filters results to accounts with four or more failures |
| convert | Converts epoch timestamps into readable time |

---

## Investigation Results

The detection query identified multiple failed authentication attempts originating from the same source IP address against a single user account within the configured five-minute detection window.

### Observations

- Multiple Failed Attempts Detected
- Source IP Address: 127.0.0.1
- Logon Type: 2 (Interactive)
- Detection Window: 5 Minutes
- Brute Force Detection Rule Triggered Successfully

---

## Failure Analysis

Additional analysis was performed to determine why authentication failed.

Observed values included:

| Field | Value |
|------|------|
| Failure Reason | An Error occurred during Logon |
| Status | 0xC000006D |
| Sub Status | 0xC0000380 |

These values confirmed that Windows rejected the authentication requests during the simulated failed logon attempts.

---

## Analyst Assessment

The observed authentication pattern satisfied the configured brute-force detection rule and generated the expected detection output.

Although the activity resembled a brute-force attack, it was intentionally generated in a controlled lab environment for security analysis and SPL query validation.

No malicious activity was present.

---

## Evidence Collected

- Failed Authentication Timeline
- Detection Rule Output
- Failure Reason Analysis
- SPL Detection Query
- Authentication Statistics

---

## Conclusion

The custom SPL detection rule successfully identified repeated failed authentication attempts by aggregating Windows Security Event ID 4625 records over a five-minute detection window.
This investigation demonstrates how Splunk Enterprise can be used to detect brute-force authentication activity using Windows Security Logs and basic statistical aggregation.

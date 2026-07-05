## SOC Project 04 — End-to-End Splunk SOC Investigation Lab


## Overview

This project demonstrates an end-to-end Security Operations Center (SOC) investigation using Splunk Enterprise and Windows Event Logs. The lab focuses on authentication monitoring, brute-force detection, Sysmon telemetry analysis, event correlation, and security investigation following a structured SOC investigation workflow.

The investigation includes failed logon analysis, successful authentication validation, privileged logons, process creation monitoring, network connection analysis, process termination events, and MITRE ATT&CK mapping.

---

# Table of Contents

- [Overview](#overview)
- [Objectives](#objectives)
- [Lab Environment](#lab-environment)
- [Tools Used](#tools-used)
- [Lab Architecture](#lab-architecture)
- [Investigation Workflow](#investigation-workflow)
- [Windows Security Log Analysis](#windows-security-log-analysis)
- [Sysmon Investigation](#sysmon-investigation)
- [Event Correlation](#event-correlation)
- [Detection Logic](#detection-logic)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Splunk Search Queries](#splunk-search-queries)
- [Screenshots](#screenshots)
- [Key Findings](#key-findings)
- [Skills Demonstrated](#skills-demonstrated)
- [Project Structure](#project-structure)
- [References](#references)

---

## Objectives

- Validate Windows Security Log ingestion into Splunk
- Validate Sysmon log ingestion
- Investigate failed authentication attempts
- Analyze successful logon events
- Detect brute-force activity
- Investigate Sysmon Process Creation events
- Investigate Sysmon Network Connection events
- Investigate Sysmon Process Termination events
- Correlate Security and Sysmon logs
- Document findings as a SOC Analyst

---

## Lab Environment

| Component | Details |
|-----------|---------|
| SIEM | Splunk Enterprise |
| Endpoint | Windows 11 |
| Log Source | Windows Security Logs |
| Telemetry | Sysmon |
| Log Forwarder | Splunk Universal Forwarder |
| Investigation Platform | Splunk Search & Reporting |

## Dataset

The investigation was performed using locally generated Windows Security Logs and Sysmon telemetry collected from a Windows 11 virtual machine. Logs were forwarded to Splunk Enterprise using the Splunk Universal Forwarder. Authentication events and endpoint telemetry were intentionally generated in a controlled lab environment for analysis.


---

## Tools Used

- Splunk Enterprise
- Splunk Universal Forwarder
- Sysmon
- Windows Event Viewer
- Windows Security Logs
- Windows 11

---

# Lab Architecture

```text
                    +-------------------------+
                    |     Windows 11 VM       |
                    |-------------------------|
                    | • Windows Security Logs |
                    | • Sysmon                |
                    +------------+------------+
                                 |
                                 | Splunk Universal Forwarder
                                 |
                                 v
                    +-------------------------+
                    |    Splunk Enterprise    |
                    |-------------------------|
                    | • Log Ingestion         |
                    | • Search & Reporting    |
                    | • SPL Queries           |
                    | • Event Correlation     |
                    +------------+------------+
                                 |
                                 v
                    +-------------------------+
                    | Security Investigation  |
                    |-------------------------|
                    | • Authentication        |
                    | • Brute Force Detection |
                    | • Sysmon Analysis       |
                    | • MITRE ATT&CK Mapping  |
                    +-------------------------+
```

### Architecture Overview

The Windows 11 system generated Windows Security and Sysmon events, which were forwarded to Splunk Enterprise using the Splunk Universal Forwarder. Splunk was used to search, analyze, and correlate the collected logs to investigate authentication activity, detect brute-force behavior, analyze endpoint telemetry, and document the investigation findings.

---

# Investigation Workflow

The investigation followed a structured process to validate log ingestion, analyze authentication activity, investigate endpoint telemetry, and correlate multiple log sources to reconstruct the observed activity.

```text
Windows Security Logs
        │
        ▼
Validate Log Ingestion
        │
        ▼
Authentication Analysis
(4625 → 4624 → 4672)
        │
        ▼
Brute Force Detection
        │
        ▼
Sysmon Analysis
(Event ID 1 → Event ID 3 → Event ID 5)
        │
        ▼
Event Correlation
        │
        ▼
MITRE ATT&CK Mapping
        │
        ▼
Investigation Findings
```

The workflow began by validating that Windows Security Logs and Sysmon events were successfully ingested into Splunk Enterprise. Authentication events were then analyzed to identify failed and successful logon attempts, followed by brute-force detection using SPL queries. Sysmon telemetry was investigated to examine process creation, network connections, and process termination. Finally, Windows Security Logs and Sysmon events were correlated to reconstruct the complete activity timeline and document the investigation findings.


---

# Windows Security Log Analysis

Windows Security events were analyzed to investigate authentication activity and determine whether the observed logons represented normal behavior or a potential security concern.

## Failed Logon – Event ID 4625

The investigation began with Event ID 4625 to identify failed authentication attempts. Multiple failed interactive logons were observed from the local system within a short period. These events were intentionally generated during the lab to simulate repeated authentication failures.

**Evidence Collected**

- Failed Logon Events
- Failed Logon Analysis

---

## Successful Logon – Event ID 4624

Following the failed logon attempts, Event ID 4624 was analyzed to determine whether authentication eventually succeeded. The investigation confirmed a successful cached interactive logon after the failed attempts.

**Evidence Collected**

- Successful Logon Details
- Successful Logon Analysis

---

## Special Privileges Assigned – Event ID 4672

Event ID 4672 was reviewed to verify whether administrative privileges were assigned after authentication. The investigation confirmed that Windows assigned the expected administrative privileges to the authenticated session.

**Evidence Collected**

- Special Privileges Assigned0


---

# Brute Force Detection

Repeated failed authentication attempts were analyzed to determine whether the observed activity matched a brute-force attack pattern. A custom SPL query was used to group failed logon events within a five-minute window and identify repeated attempts against the same account.

## Detection Logic

The detection rule monitored Windows Security Event ID 4625 and generated an alert when four or more failed logon attempts were recorded for the same account within a five-minute period.

## Investigation Summary

The investigation identified four consecutive failed interactive logon attempts originating from the same source IP address. The activity matched the configured detection rule and demonstrated how repeated authentication failures can be identified using Splunk SPL.

## Evidence Collected

- Brute Force Detection
- Failure Reason Analysis
- Attack Timeline
- Detection Rule


---

# Sysmon Investigation

Sysmon telemetry was analyzed to investigate endpoint activity after authentication. The investigation focused on process creation, network communication, and process termination to better understand system behavior.

## Event ID 1 – Process Creation

Sysmon Event ID 1 was reviewed to identify newly created processes. The investigation confirmed that **notepad.exe** was launched by **explorer.exe**, indicating normal user-driven execution through the Windows graphical interface.

**Evidence Collected**

- Process Creation Details
- Process Creation (Notepad)

---

## Event ID 3 – Network Connection

Sysmon Event ID 3 was analyzed to identify processes initiating network connections. The observed activity showed **chrome.exe** communicating over UDP port **5353** using IPv6 multicast addresses, which is consistent with Multicast DNS (mDNS) traffic.

**Evidence Collected**

- Network Connection Details
- Network Connection Search

---

## Event ID 5 – Process Termination

Sysmon Event ID 5 was investigated to verify process termination and correlate it with the previously observed process creation event. The matching Process ID and Process GUID confirmed the complete lifecycle of the process.

**Evidence Collected**

- Process Termination Search
- Process Termination Details


---

# Event Correlation

Individual events provide useful information, but correlating multiple log sources gives a more complete understanding of system activity. During this investigation, Windows Security Logs and Sysmon events were correlated to reconstruct the sequence of actions observed in the lab.

## Correlation Timeline

| Time | Event |
|------|-------|
| 10:56:15 AM | Event ID 4625 – Failed Logon |
| 10:56:38 AM | Event ID 4624 – Successful Logon |
| 10:56:38 AM | Event ID 4672 – Administrative Privileges Assigned |
| 10:57:24 AM | Sysmon Event ID 1 – Process Creation |
| 10:57:52 AM | Sysmon Event ID 3 – Network Connection |
| 10:58:00 AM | Sysmon Event ID 5 – Process Termination |

## Investigation Outcome

By correlating authentication events with Sysmon telemetry, the complete sequence of activity was reconstructed. The investigation confirmed a controlled lab scenario consisting of repeated failed logon attempts, successful authentication, administrative privilege assignment, normal process execution, network communication, and process termination. No indicators of malicious activity were identified.


---

# MITRE ATT&CK Mapping

The observed activities were mapped to the MITRE ATT&CK framework where appropriate. Only behaviors supported by the collected evidence were mapped to avoid associating normal Windows activity with adversary techniques.

| Technique ID | Technique | Evidence |
|--------------|-----------|----------|
| T1110 | Brute Force | Multiple failed authentication attempts (Event ID 4625) |
| T1078 | Valid Accounts | Successful authentication following repeated failed logon attempts (Event ID 4624) |

**Note:** Event ID 4672 and the observed Sysmon events were not mapped because they represented expected operating system behavior during the lab and did not indicate malicious activity.

---

# Splunk Search Queries

The repository includes the SPL queries used throughout the investigation.

- Authentication Investigation Queries
- Brute Force Detection Queries
- Sysmon Investigation Queries

These queries demonstrate how Splunk Search Processing Language (SPL) was used to investigate authentication events, detect repeated failed logons, analyze Sysmon telemetry, and correlate multiple log sources.

---

# Key Findings

- Successfully validated Windows Security Log ingestion into Splunk.
- Successfully validated Sysmon log ingestion into Splunk.
- Investigated failed and successful authentication events.
- Detected repeated failed logon attempts using a custom SPL detection rule.
- Investigated Sysmon Process Creation, Network Connection, and Process Termination events.
- Correlated Windows Security Logs with Sysmon telemetry to reconstruct the investigation timeline.
- Mapped observed activity to the MITRE ATT&CK framework where appropriate.

---

# Skills Demonstrated

- Security Event Log Analysis
- Splunk Search Processing Language (SPL)
- Authentication Investigation
- Brute Force Detection
- Windows Security Log Analysis
- Sysmon Investigation
- Event Correlation
- Threat Detection
- MITRE ATT&CK Mapping
- Security Investigation Documentation

---

# Project Structure
```
SOC-Project-4-Splunk-SOC-Investigation-Lab
│
├── Documentation
│   ├── 01_Windows_Security_Log_Analysis.md
│   ├── 02_Brute_Force_Detection.md
│   ├── 03_Sysmon_Investigation.md
│   ├── 04_Event_Correlation.md
│   └── 05_MITRE_ATTACK_Mapping.md
│
├── Queries
│   ├── 01_Authentication_Queries.spl
│   ├── 02_Brute_Force_Queries.spl
│   └── 03_Sysmon_Queries.spl
│
├── Screenshots
│   ├── Authentication
│   ├── Bruteforce
│   ├── Correlation
│   └── Sysmon
│
└── README.md
```
---

# Screenshots

The repository contains supporting screenshots demonstrating:

- Windows Security Event ID 4625 (Failed Logon)
- Windows Security Event ID 4624 (Successful Logon)
- Windows Security Event ID 4672 (Special Privileges Assigned)
- Sysmon Event ID 1 (Process Creation)
- Sysmon Event ID 3 (Network Connection)
- Sysmon Event ID 5 (Process Termination)
- Brute Force Detection
- Authentication Timeline
- Event Correlation

---

# References

- Microsoft Windows Security Auditing
- Microsoft Sysmon
- Splunk Enterprise Documentation
- MITRE ATT&CK Framework

---

## Conclusion

This project demonstrates a complete SOC investigation workflow using Splunk Enterprise by combining Windows Security Logs and Sysmon telemetry. The investigation covered authentication analysis, brute-force detection, endpoint activity monitoring, event correlation, and MITRE ATT&CK mapping. It strengthened practical skills in log analysis, SPL query development, investigation documentation, and structured incident analysis while reinforcing the importance of correlating multiple data sources during security investigations.


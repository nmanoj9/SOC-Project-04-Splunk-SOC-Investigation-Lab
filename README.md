# SOC Project 04 – Splunk SOC Investigation Lab

## Overview

This project demonstrates an end-to-end Security Operations Center (SOC) investigation using Splunk Enterprise and Windows Event Logs. The lab focuses on authentication monitoring, brute-force detection, Sysmon telemetry analysis, event correlation, and security investigation from the perspective of a SOC Analyst.

The investigation includes failed logon analysis, successful authentication validation, privileged logons, process creation monitoring, network connection analysis, process termination events, and MITRE ATT&CK mapping.

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

---

## Tools Used

- Splunk Enterprise
- Splunk Universal Forwarder
- Sysmon
- Windows Event Viewer
- Windows Security Logs
- Windows 11



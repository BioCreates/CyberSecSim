# Threat Hunting — RDP Brute Force Detection

## Overview

This lab demonstrates how to detect potential RDP brute-force attacks in a Windows environment using Sigma rules and local PowerShell testing.
The goal is to:

1. Identify failed RDP logon attempts (Event ID 4625, Logon Type 10)
2. Aggregate failures from the same IP within a short timeframe
3. Validate detection logic without a SIEM

---

## What is Sigma?

Sigma is a vendor-neutral rule format for writing log-based detections.
Once written, a Sigma rule can be converted into the query language of your preferred SIEM (Splunk, Elastic, Sentinel, etc.) using the Sigma CLI or online tools.

---

## Detection Logic

Indicators of RDP brute-force:

* Multiple failed logons (Event ID 4625)
* Logon Type 10 (Remote Interactive / RDP)
* Same source IP over a short period

---

### Sigma Rule

```yaml
title: Possible RDP Brute Force - Multiple 4625 From Single IP
id: rdp-bruteforce-agg-roninsec-v1
status: experimental
description: Flags potential RDP brute force based on repeated failed logons (Event ID 4625) from the same source IP within a short timeframe.
logsource:
  product: windows
  service: security
detection:
  selection:
    EventID: 4625
    LogonType: 10
  timeframe: 10m
  condition: selection | count(IpAddress) by 1 >= 5
fields:
  - IpAddress
  - TargetUserName
  - WorkstationName
falsepositives:
  - User mistyping password repeatedly
level: medium
tags:
  - attack.t1110
```

---

## Local Testing Without a SIEM

We validated the rule logic with a PowerShell script that:

* Queries Security logs for Event ID 4625 in the last 10 minutes
* Filters for Logon Type 10
* Groups results by IP address
* Displays IPs with 5 or more failures

Script: [`SigmaShell.ps1`](./artifacts/SigmaShell.ps1)
Example Output:

```
Name            Count
----            -----
192.168.1.105   10
```

---

## Repository Structure

```
.
├── artifacts/      # Sigma rule, PowerShell script, converted queries
├── logs/           # Placeholder for exported event logs
├── screenshots/    # Test results and reference images
├── attack.md       # Attack simulation notes
├── detection.md    # Detection logic and validation steps
└── overview.md     # Project overview
```

---

## How to Reproduce

1. Simulate: Trigger multiple failed RDP login attempts from a test IP
2. Collect Logs: Ensure Windows Security Auditing is enabled
3. Run Script: Execute `SigmaShell.ps1` to check for matching events
4. (Optional): Convert Sigma rule to your SIEM’s query language

---

## Next Steps

* Integrate rule into Wazuh or Elastic for live monitoring
* Add detection for successful logons after brute-force sequences
* Tune thresholds for your environment

---

## References

* [SigmaHQ](https://github.com/SigmaHQ/sigma)
* [MITRE ATT\&CK — Brute Force (T1110)](https://attack.mitre.org/techniques/T1110/)

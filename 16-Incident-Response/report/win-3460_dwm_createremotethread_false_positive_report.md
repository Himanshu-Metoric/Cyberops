# Incident Report — False Positive CreateRemoteThread

## Incident ID
INC-20260613-02

## Executive Summary
A Sysmon alert triggered on 13 June 2026 for a `CreateRemoteThread` event involving the legitimate Windows process `dwm.exe` on host `win-3460`. The investigation found normal host activity and no supporting indicators of malicious behavior, so this incident is classified as a false positive.

## Key Findings
- Alert rule: `CreateRemoteThread`
- Host: `win-3460`
- Affected user: `Roger Fedora`
- Department: `Marketing`
- Process name: `dwm.exe`
- Process ID: `3772`
- Data source: `Sysmon`
- No suspicious PowerShell or shell activity
- No malicious DNS requests or external connections
- No evidence of malware execution or persistence mechanisms

## What Happened?
A Sysmon alert was generated for a `CreateRemoteThread` event involving `dwm.exe` on host `win-3460`.

## What Was Investigated?
- Reviewed Sysmon events around the alert timestamp
- Analyzed process creation and parent-child relationships
- Checked DNS activity and network behavior
- Looked for suspicious indicators such as:
  - malicious PowerShell execution
  - external network connections
  - malicious DNS requests
  - reverse shell activity
  - unusual command-line arguments

## What Was Found?
- `MoUsoCoreWorker.exe` executed from the legitimate Windows `System32` directory
- `iexplore.exe` launched from the standard Internet Explorer path with normal parent-child behavior
- `OUTLOOK.EXE` queried `mailsrv-01.tryhatme.com` and resolved to internal IP `172.16.1.15`
- `taskhostw.exe` was executed by `svchost.exe` from the legitimate Windows directory
- `spoolsv.exe` modified a printer-related registry key consistent with normal printing activity
- No suspicious PowerShell, malware execution, external C2, or malicious DNS activity was identified
- The `dwm.exe` event lacked further malicious context and appeared consistent with normal Windows behavior

## Classification
- Classification: False Positive
- Escalation Required: No

## Reason for Classifying as False Positive
The alert was triggered by a `CreateRemoteThread` event involving legitimate Windows process `dwm.exe`, but no additional evidence of compromise was found. Surrounding host activity was consistent with normal Windows and user behavior, and there were no supporting indicators of malicious activity.

## Recommended Actions
- Monitor `win-3460` and the affected user for follow-up alerts
- Review detection logic for the `CreateRemoteThread` rule to reduce similar false positives
- Validate whether the alert should be refined to exclude benign `dwm.exe` activity
- Document the false positive as feedback for future tuning

## Closure
The incident is closed as a false positive due to lack of malicious evidence and normal host activity.

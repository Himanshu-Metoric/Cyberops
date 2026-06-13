# Incident Report — win-3450 PowerView Compromise

## Incident ID
2026-06-13-WIN3450-IR01

## Executive Summary
On 13 June 2026, host `win-3450` was confirmed as a true positive compromise. Forensic telemetry identified execution of `PowerView.ps1` from `C:\Users\michael.ascot\Downloads`, followed by Active Directory enumeration and ACL discovery commands. The incident was escalated to Incident Response for containment and further investigation.

## Key Findings
- Malicious artifact: `C:\Users\michael.ascot\Downloads\PowerView.ps1`
- Evidence of post-exploitation: PowerShell scriptblock logging captured PowerView functions including `Find-GPOComputerAdmin`, `Invoke-ACLScanner`, and `Get-NetComputer`
- Observed behavior inconsistent with legitimate admin operations
- Compromised user account: `michael.ascot`

## Impact
- Confirmed host compromise on `win-3450`
- Potential domain enumeration and privilege escalation reconnaissance
- Elevated risk to Active Directory and other enterprise assets if attacker activity continues

## Containment Actions Completed
- Host isolation via EDR / network quarantine
- Evidence preservation: Sysmon and PowerShell logs, file artifacts, live process snapshot
- Credential containment: disable and reset compromised account, revoke Kerberos tickets
- Notification to IR lead, SOC manager, EDR engineer, and AD admin

## Recommended Next Steps
- Conduct full forensic capture of `win-3450` (memory and disk)
- Review domain controller and other endpoint logs for related PowerView usage
- Hunt for additional instances of PowerView and similar offensive tooling
- Harden PowerShell logging and enforcement on endpoint controls
- Review access and ACL changes for any lateral movement or replication abuse

## Evidence Collected
- Sysmon events: 1, 3, 11 for `win-3450`
- PowerShell ScriptBlock logs (4104)
- PowerView script artifact from `C:\Users\michael.ascot\Downloads`
- Timeline summary and SIEM query output

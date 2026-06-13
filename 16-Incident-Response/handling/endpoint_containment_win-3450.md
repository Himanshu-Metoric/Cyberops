# Endpoint Containment & Response — win-3450

## Objective
Contain and investigate an active PowerView post-exploitation incident on host `win-3450`, preserve evidence, and limit further domain exposure.

## Scope
- Host: `win-3450`
- User: `michael.ascot`
- Observed activity: PowerShell execution of PowerView and AD enumeration

## Response Steps
1. Quarantine the host immediately via EDR network isolation.
2. Preserve volatile evidence before remediation:
   - Memory image
   - Active process snapshot
   - Sysmon and PowerShell logs
   - Copies of `C:\Users\michael.ascot\Downloads\PowerView.ps1`
3. Capture host-specific artifacts:
   - Sysmon event codes 1, 3, 11
   - PowerShell scriptblock logs (4104)
   - File creation events and process command lines
4. Identify and terminate malicious sessions safely:
   - Target `powershell.exe` PID(s) associated with PowerView
   - Avoid disrupting evidence collection until captured
5. Reset compromised credentials:
   - Disable and reset `michael.ascot`
   - Revoke Kerberos tickets and live sessions
6. Notify stakeholders:
   - Incident Response Lead
   - SOC Manager
   - EDR engineer
   - Active Directory administrator

## Investigation Checklist
- Review SIEM query results for host `win-3450` and correlate with other hosts
- Search for the same PowerView indicators across the environment
- Check for lateral movement or AD abuse beyond the initial host
- Identify whether any additional accounts were targeted or used

## Follow-up Actions
- Full forensic collection and chain-of-custody documentation
- Privileged account review and password resets if needed
- PowerShell logging hardening and PowerView / offensive tooling detection
- Review Microsoft Active Directory ACLs and genealogy of any suspicious changes

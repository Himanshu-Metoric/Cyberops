# ZTA SOC Playbook — High Level

## Purpose
Actions for the SOC when validating Zero Trust-related alerts and incidents.

## Playbook: Suspicious Access Attempt
1. Alert triage: collect user, device, app, location, timestamp
2. Verify identity with IAM logs (MFA success/failure, SSO session)
3. Check device posture (EDR, patching, secure boot)
4. Check policy engine decision and enforcement logs (ZTNA gateway)
5. If denied: record and close after watch period
6. If allowed but risky: force step-up authentication, isolate session
7. If evidence of compromise: escalate to IR, isolate host, collect forensic data

## Playbook: Lateral Movement Detection
1. Identify initial and target assets and segments
2. Query network flows and microsegment logs
3. Block offending flows at enforcement point
4. Hunt for other similar access from same account or host
5. Escalate to IR if suspected compromise

## Playbook: Device Non-Compliance
1. Identify failing checks (EDR offline, patches missing)
2. Quarantine device or restrict access to critical resources
3. Notify asset owner and IT to remediate
4. Reassess post-remediation

## Notes
- Map playbook steps to SIEM queries and automation runbooks
- Keep runbooks short and scriptable
- Add links to exact SIEM queries and blocklist procedures in your environment

# Zero Trust Architecture (ZTA) — Primer

TL;DR
- Zero Trust = never trust, always verify. Authenticate and authorize every request.
- Core principles: least privilege, microsegmentation, continuous verification, assume breach.

Audience: SOC, IR, architects, identity teams.

---

## 1. What is Zero Trust?
Zero Trust Architecture (ZTA) is a security strategy that treats every user, device, and application as untrusted by default. Access is granted per-request after continuous verification of identity and device posture.

## 2. Why it matters
Cloud, remote work, and BYOD erased the old perimeter. ZTA reduces attack surface, limits lateral movement, and helps meet compliance.

## 3. Core Principles
- Never trust, always verify
- Least privilege
- Microsegmentation
- Continuous monitoring and analytics
- Assume breach

## 4. Key Components
- Identity & Access Management (IAM): MFA, SSO, centralized identity
- Device Posture: OS version, patch status, EDR health
- Policy Engine: real-time contextual access decisions
- Microsegmentation: isolate workloads and services
- Monitoring & Analytics: logging, UEBA, threat intel

## 5. Implementation steps (high level)
1. Inventory assets and map workflows
2. Harden identity: enforce MFA and SSO
3. Establish device posture checks
4. Define least-privilege policies and microsegments
5. Implement policy engine and enforcement points (ZTNA, CASB, network controls)
6. Monitor, test, and iterate

## 6. SOC use cases and detection rules
- Verify anomalous access (user odd hour, unfamiliar location)
- Block risky devices failing posture checks
- Monitor for lateral movement between segments
- Enforce step-up authentication for sensitive data access

## 7. Quick checklist
See `zero_trust_checklist.md` for a short actionable checklist.

## 8. Mermaid diagram
```mermaid
flowchart LR
  A[User] --> B[IAM + MFA]
  B --> C[Policy Engine]
  C --> D{Decision: Allow / Deny}
  D -->|Allow| E[Access Enforcer (ZTNA/CASB)]
  E --> F[Microsegmented Resource]
  E --> G[Cloud App]
  style A fill:#f9f,stroke:#333,stroke-width:1px
```

## 9. Recommended next actions
- Run an identity and device posture gap assessment
- Prioritise high-value assets for microsegmentation
- Add ZTA detection rules to SOC playbooks

---

*Stored in both `14-GRC/Identity-and-Access` and `28-Resources/Reference` for visibility.*

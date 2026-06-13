# Email False Positive Triage

## Objective
Document the triage of a false positive email alert and capture tuning recommendations to reduce future noise.

## Incident Summary
- Alert type: Email gateway / SIEM alert
- Disposition: False Positive
- Affected recipient: yani.zubair@tryhatme.com
- Sender: leonard@fashionindustrytrends.xyz
- Trigger: heuristic match on `.xyz` TLD

## Investigation Findings
- No attachments present
- No URLs detected in the email body
- Content matched marketing/spam wording
- Domain reputation and phishing indicators did not support escalation

## Decision Rationale
- The alert was driven by an over-sensitive TLD rule rather than confirmed malicious behavior
- Evidence did not show credential harvesting, active links, or delivery of a weaponized payload

## Tuning Action Items
- Adjust SIEM scoring so unusual TLDs contribute to a risk score instead of generating standalone alerts
- Add secondary trigger conditions such as:
  - SPF/DKIM failure
  - Domain age < 14 days
  - Known bad reputation or threat intelligence match
- Mark `fashionindustrytrends.xyz` as unsolicited bulk email for the gateway filtering policy
- Monitor for repeated alerts from the same sender domain and apply automated quarantine if volume spikes

## Closure Notes
The alert is closed as a false positive with documented reasoning and recommended tuning to reduce similar false positives in the future.

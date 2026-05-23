# Security Policy

## Overview

Interview Ready is a Claude skill package consisting of markdown instruction files. It does not include executable server code, databases, authentication systems, or network services. The attack surface is intentionally minimal.

That said, security matters here for specific reasons. This package handles sensitive personal information , career history, compensation targets, employment status, and job search motivations. It also generates guidance that job seekers will act on in real situations. Getting that wrong has real consequences for real people.

---

## A Note on AI-Generated Content

This package generates career guidance using Claude. The output quality depends on the quality of the skill instructions and the information the user provides. No AI-generated guidance should be treated as legal, financial, or professional advice without independent verification. The disclaimer in the README applies to all outputs from this package.

---

## Supported Versions

This package does not use semantic versioning. The current state of the `main` branch is the only supported version. Security issues are addressed there only.

| Version | Supported |
|---|---|
| main (current) | Yes |
| Any prior commit | No |

---

## What to Report

Please report any of the following:

**Data exposure risks**
Anything in the skill files that could cause Claude to expose, store, or transmit a user's personal information in an unintended way. This includes prompt patterns that could cause the profile JSON to be surfaced inappropriately.

**Prompt injection vulnerabilities**
Patterns in the skill instructions that could be exploited through malicious job descriptions, resume content, or other user-supplied input to cause Claude to take unintended actions.

**Misleading or harmful guidance**
Content in any skill file that gives job seekers advice that is factually wrong, legally incorrect, or likely to cause harm in a real job search situation. This includes outdated salary law information, incorrect EEOC guidance, or negotiation advice that could damage a candidate's position.

**License violations in contributions**
If you identify contributed content that appears to violate the CC BY-NC 4.0 license, contains copyrighted material used without permission, or includes real personal data from identifiable individuals, report it.

---

## What Not to Report Here

- Bugs in Claude itself or the Claude.ai platform , report those to Anthropic
- General feature requests or quality issues , use the bug report or feature request issue templates
- Questions about how the skills work , use GitHub Discussions

---

## How to Report

**Do not open a public issue for security vulnerabilities.**

Report security issues privately by:

1. Going to the Security tab in this GitHub repo
2. Clicking "Report a vulnerability" to open a private advisory

If the Security tab is not available, contact @ungatedbluemarble directly through GitHub.

Include in your report:

- Which skill or file is affected
- A description of the vulnerability or risk
- Steps to reproduce or demonstrate the issue if applicable
- Your assessment of the severity and potential impact

---

## Response Commitment

Security reports will be acknowledged within 7 days. Resolution timeline depends on severity. Critical issues affecting user safety or data exposure will be prioritized above all other work.

---



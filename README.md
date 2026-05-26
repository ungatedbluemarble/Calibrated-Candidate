# Calibrated Candidate: Claude Skill Package

Every other tool gives you a service and costs money. Calibrated Candidate gives you a system that knows you, researches for you, and works across your entire job search from first resume to signed offer.

A six-skill package that takes any job seeker from raw profile to hired. Covers onboarding, resume writing, job search evaluation, full interview pipeline preparation, live EEOC-compliant mock interview sessions, and pipeline tracking.

Works in Claude.ai (browser), Claude Desktop, and Claude Code. Calibrated Candidate is always free.

---

## User Guide

New to Calibrated Candidate? The [User Guide](https://github.com/ungatedbluemarble/calibrated-candidate/blob/main/CalibratedCandidate_UserGuide_2026.pdf) covers both Claude free and Pro plan users and walks you through every skill session by session, from your first profile build through offer negotiation. No technical knowledge required.

---

## What This Is

Calibrated Candidate is a structured, research-grounded career preparation system built as Claude skills. Each skill is autonomous but context-aware: they share a user profile so you are never asked the same question twice across the workflow.

Every output is grounded in actual company research and your real background. This is not a template generator.

---

## The Six Skills

| Skill | What It Does |
|---|---|
| `skill-01-interviewer` | Onboards the user. Builds the shared profile: background, strengths, weaknesses, job search status and motivation. Asks about connected email, storage, and calendar services so downstream skills can read inbox responses and pull documents automatically. The starting point for every other skill. |
| `skill-02-resume-writer` | Accepts your resume via file upload or any document source. Rewrites or builds your resume and cover letter in professional, human language. Primary output is Word (.docx). |
| `skill-03-job-search` | Matches your profile to industries and roles. Evaluates job descriptions you bring: individually or in bulk. Tells you whether to apply and how to position. |
| `skill-04-interview-prep` | Prepares you for every stage of the hiring pipeline: recruiter screen, hiring manager, panel, final round, and offer negotiation. Research-grounded, role-specific, candidate-anchored prep documents. Includes reference preparation, follow-up cadence, and resignation handling. |
| `skill-05-mock-interviewer` | Runs a live, interactive mock interview session tailored to a specific job description and the candidate's real background. Three modes: standard practice, deep simulation, and quiz mode. Every answer graded against an EEOC-compliant standard. |
| `skill-06-pipeline-dashboard` | Tracks your active job search pipeline. If your email is connected and recruiter contacts are stored, checks your inbox for responses before rendering the dashboard so status reflects what actually happened, not just what you manually logged. Flags what needs attention and routes to prep or practice without loading a full prep session. |

---

## Requirements

- Claude.ai account (free plan or paid) or Claude Code
- Code Execution enabled: Settings > Capabilities > Code Execution (required for skills to load)
- No API key required. You bring your own Claude environment.

---

## Installation

### Option 1: Claude.ai or Claude Desktop (no technical setup required)

This is the path for most users.

**Step 1: Download the package**

On this GitHub page, click the green **Code** button, then click **Download ZIP**. Save the file to your computer.

**Step 2: Open Claude settings**

- In Claude.ai (browser): go to [claude.ai](https://claude.ai), click your profile icon in the top right, select **Settings**, then click **Features** in the sidebar, then **Skills**.
- In Claude Desktop: open the app, go to **Settings > Capabilities > Skills**.

**Step 3: Enable Code Execution**

In Settings > Capabilities, make sure **Code Execution** is turned on. Skills will not load without it.

**Step 4: Upload each skill**

Each skill is a separate folder inside the ZIP you downloaded. You need to upload them one at a time.

For each of the six skill folders (Interviewer, Resume Writer, Job Search Expert, Interview Prep, Mock Interviewer, Pipeline Dashboard):

1. Create a new ZIP file containing just that one skill folder
2. Click **Upload skill** in the Skills settings panel
3. Select the ZIP file and upload it
4. Repeat for each skill

Once all six are uploaded, Claude will use them automatically when you start a job search session.

**How to zip a single skill folder (no coding required):**
- On Mac: right-click the skill folder > Compress
- On Windows: right-click the skill folder > Send to > Compressed (zipped) folder

---

### Option 2: Claude Code (command line)

If you use Claude Code, clone the repo and copy the skills to your skills directory:

```bash
git clone https://github.com/ungatedbluemarble/calibrated-candidate.git
cp -r calibrated-candidate/skill-* ~/.claude/skills/
```

Or install per-project (shared with anyone who clones your repo):

```bash
cp -r calibrated-candidate/skill-* .claude/skills/
```

---

### Option 3: Direct GitHub install (Claude Code one-liner)

```bash
mkdir -p ~/.claude/skills && \
  git clone https://github.com/ungatedbluemarble/calibrated-candidate.git /tmp/calibrated-candidate && \
  cp -r /tmp/calibrated-candidate/skill-* ~/.claude/skills/
```

---

## Usage

Once installed, start with the Interviewer skill. Everything else follows from there.

In any Claude session, say:

> "I'm ready to start Calibrated Candidate."

or

> "I need help with my job search."

The Interviewer skill will onboard you, build your profile, and route you to the right skill based on what you need.

---

## Skill Trigger Reference

You can also go directly to any skill:

| What to say | Skill triggered |
|---|---|
| "Start Calibrated Candidate" / "I need help with my job search" | Interviewer |
| "Write my resume" / "Update my cover letter" | Resume Writer |
| "Find jobs for me" / "Review this job description" | Job Search Expert |
| "I have an interview" / "Prep me for my interview" | Interview Prep |
| "Mock interview" / "Practice with me" / "Interview me" / "Quiz me on the role" | Mock Interviewer |
| "Show me my pipeline" / "Where am I with my applications" / "Add this to my tracker" | Pipeline Dashboard |

---

## Connected Services (Optional)

Calibrated Candidate can connect to your email, cloud storage, and calendar through Claude's connected apps to make the workflow significantly more useful. These are optional: everything works without them.

During onboarding (Interviewer), you will be asked whether you want to enable any of the following:

| Service | What it enables |
|---|---|
| Gmail or Microsoft Outlook | Pipeline Dashboard checks your inbox for recruiter responses before showing your pipeline. Status reflects what actually happened, not just what you manually logged. |
| Google Drive, OneDrive, SharePoint, Dropbox, Box | Resume Writer and Job Search Expert can pull your resume or job descriptions directly from storage without requiring a file upload each time. |
| Google Calendar or Outlook Calendar | Pipeline Dashboard can read scheduled interview dates from your calendar and surface them in the pipeline view automatically. |

To connect a service, go to Claude settings and enable the relevant connector under Connected Apps before starting your Calibrated Candidate session. You can add connectors at any time: if you connect email after your initial onboarding, tell the Interviewer skill during your next session and it will update your profile.

**Recruiter email addresses** are the key to inbox checking. When you run prep for any role (Interview Prep), you will be prompted to share the recruiter's email if you have it. Once stored in your profile, Pipeline Dashboard uses it to find the right email thread when checking your pipeline status.

---

You can feed your resume and other career documents to this package from any source:

- **Direct upload**: drag and drop a file into the Claude chat window
- **Copy and paste**: paste the text of a resume, job description, or any other document directly into chat
- **Cloud storage**: if you have a cloud storage connector enabled in Claude (Google Drive, OneDrive, Dropbox, Box, SharePoint, or any other connected service), you can reference files stored there directly
- **Local files**: in Claude Code or Claude Desktop, you can reference files stored anywhere on your computer
- **Any format**: PDF, Word (.docx), plain text (.txt), or pasted text all work

The skills do not require any specific cloud service. Whatever document source you have access to works.

---

## Pipeline Dashboard

Calibrated Candidate includes a purpose-built pipeline dashboard through the `skill-06-pipeline-dashboard` skill. You do not need an external tracker.

The dashboard surfaces your full job search in one view: active applications by stage, what needs attention and why, upcoming interview dates, offers pending, and last recruiter contact for each role. It flags urgency automatically. If a final round is in two days with no prep completed, it surfaces that. If a hiring manager interview went quiet nine days ago, it flags it for follow-up. If an offer is pending, it prompts you to run negotiation prep before responding.

If your email is connected, the dashboard checks your inbox before rendering so the status of every application reflects what actually happened, not just what you manually logged.

To open the dashboard, say:

> "Show me my pipeline."

or

> "Where am I with my applications."

To update a specific application, say what changed and the dashboard refreshes that entry. Say "save my profile" after any update to keep the pipeline current across sessions.

---

## How Context Works

The user profile built in the Interviewer skill carries forward into every other skill automatically. Skills pull only the slice of context they need: you are never re-asked for information you already provided.

**Session-persistent (default):** The profile lives in the active conversation. No file is written unless you request it. If you close the session, the profile does not carry over automatically.

**File-based (portable):** Export your profile at any time by saying "save my profile." It saves as `calibrated_candidate_profile.json`. In future sessions, upload that file and any skill reloads your full profile without re-interviewing you.

**Performance note:** Long sessions with heavy document processing: multiple resume revisions, several prep documents: may approach Claude's context limits. If that happens, export your profile, start a new session, upload the file, and continue. The skill will pick up where you left off.

---

## Document Outputs

| Document | Skill | Format |
|---|---|---|
| Resume | Resume Writer | .docx (primary), PDF or plain text on request |
| Resume Before and After | Resume Writer | .html (on request after rewrite) |
| Cover Letter | Resume Writer | .docx (primary) |
| Recruiter Screen Prep Guide | Interview Prep | .docx |
| Hiring Manager Prep Guide | Interview Prep | .docx |
| Panel Interview Prep Guide | Interview Prep | .docx |
| Final Round Prep Guide | Interview Prep | .docx |
| Offer Negotiation Guide | Interview Prep | .docx |
| Compensation Analysis Report | Interview Prep | .html (on request after benchmarking) |
| Reference Preparation Guide | Interview Prep | Inline guidance, reference list template |
| Follow-Up Cadence Guide | Interview Prep | Inline guidance, email templates by stage |
| Resignation Handling Guide | Interview Prep | Inline guidance, resignation letter template |
| Pipeline Dashboard | Pipeline Dashboard | .html (on request, generated once per session) |

---

## Disclaimer

Calibrated Candidate is provided for informational purposes only and does not constitute legal, financial, or professional career advice. Compensation data, salary law guidance, and negotiation strategies are research aids, not legal or financial counsel. Use your own judgment and consult a qualified professional where appropriate.

---

## A Note on Salary Law Intelligence

Interview Prep includes a live salary law search that runs on first use and refreshes automatically every 90 days (one US fiscal quarter). When you reach the compensation section of any recruiter screen prep, the skill searches for current salary history and expectation ban laws in your state and city, summarizes what applies to you, and cites the source.

This search is live, not static. The skill does not rely on a fixed list that ages out. It pulls current information, stores the result in your profile, and tells you when it will refresh next.

The result is always accompanied by a disclaimer directing you to your state labor department to verify, because laws change and a web search is not a substitute for official guidance. The skill makes this clear every time.

If you are outside the United States, the skill runs a country and city-specific search instead and flags when reliable information could not be confirmed.

---

## A Note on the Shared Profile Schema

The user profile schema (`user-profile-schema.md`) is intentionally duplicated into the `references/` folder of each skill. This is required because each skill is installed as a separate zip file in Claude and cannot access files outside its own folder. When the schema changes, update all six copies. The `shared/` folder at the repo root is the source of truth, copy from there.

---

## A Note on LinkedIn

LinkedIn blocks automated content fetching. When this package researches a hiring manager, it extracts their name and company from the URL you provide and runs web search queries instead, synthesizing results from public sources: posts, articles, events, and announcements. If minimal public data exists for an interviewer, the skill flags it and focuses on role-level signals instead.

---

## Managing Your Session

Calibrated Candidate is designed to be used across multiple sessions, not in one sitting. Each skill is token-intensive and running the full pipeline end to end in a single conversation will approach Claude's context limits.

**Estimated token usage by stage:**

| Stage | Estimated Tokens |
|---|---|
| Interviewer: Intake and profile build | 2,000 to 4,000 |
| Resume Writer: Resume rewrite and cover letter | 8,000 to 15,000 |
| Job Search Expert: Single JD evaluation | 3,000 to 5,000 |
| Job Search Expert: Bulk JD review (5 JDs) | 10,000 to 15,000 |
| Interview Prep: Single stage prep document | 8,000 to 12,000 |
| Mock Interviewer: Standard practice session | 4,000 to 8,000 |
| Mock Interviewer: Deep simulation session | 6,000 to 12,000 |
| Pipeline Dashboard: Dashboard and updates | 1,000 to 2,000 |

**Full pipeline estimate:** A complete end-to-end run covering intake, resume, one JD evaluation, recruiter screen prep, hiring manager prep, and a mock interview session consumes roughly 35,000 to 55,000 tokens. Running this in one session is possible but not recommended.

**Recommended approach:**

Run one skill per session. Save your profile at the end of every session by saying "save my profile." Upload the saved file at the start of the next session to pick up exactly where you left off.

Suggested session breakdown:

Session 1: Interviewer intake and profile build. Save profile.

Session 2: Resume Writer: resume and cover letter. Save profile.

Session 3: Job Search Expert: job search and JD evaluation for your top roles. Save profile.

Session 4: Interview Prep: recruiter screen prep for your priority role. Mock Interviewer: practice session. Save profile.

Session 5: Interview Prep: hiring manager prep when the screen goes well. Save profile.

Continue as needed through panel, final round, and offer negotiation.

**If your session runs out of context:** Say "save my profile" immediately, start a new session, upload the profile file, and continue. The skill will pick up where you left off.

---

## Troubleshooting

**Skills are not triggering**
Make sure Code Execution is enabled in Settings > Capabilities. Skills will not load without it.

**Document upload is not working**
Any file format Claude supports works: PDF, Word, or plain text. If a file fails to upload, paste the text directly into chat instead.

**Session ran out of context**
Say "save my profile" before the session ends. Upload the saved file in a new session to continue without losing your work.

---

## Contributing

Pull requests welcome. If you extend this package for a specific industry, role type, or additional platform support, open a PR with your additions and a note on what changed and why.

---

## License

Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0).

You are free to use, share, and adapt this work for non-commercial purposes as long as you credit @ungatedbluemarble as the original creator and indicate any changes made. Commercial use of any kind is prohibited.

Full license: https://creativecommons.org/licenses/by-nc/4.0/

---

*Built by @ungatedbluemarble. Calibrated Candidate was built from a real workflow and open-sourced because the problem is universal.*

---

## Compensation Research

When preparing for offer negotiation, Interview Prep automatically searches these sources and synthesizes them into a base, bonus, equity, and total compensation range for your specific role, level, and location. These sources are also listed so you can cross-reference independently.

| Source | Best For |
|---|---|
| Levels.fyi | Verified total compensation data for technology roles, especially engineering and product |
| Glassdoor | Broad cross-industry self-reported ranges. Note: skews toward base salary, may undercount total comp |
| LinkedIn Salary | Role and location-specific ranges tied to real job postings |
| Payscale | Detailed breakdown by experience, education level, and geography. Strong for non-technical roles |
| Bureau of Labor Statistics (bls.gov) | Authoritative US government occupational wage data. Best for anchoring the low end of a range |
| Dice | Technology and engineering roles. Strong for software, data, and infrastructure positions |
| Built In | Startup and growth-stage company compensation. Covers equity and total comp packages |
| Indeed Salary | High volume, broad coverage across industries. Best used as a cross-reference |

## Salary Law Research

Interview Prep also searches these authoritative sources for salary history and expectation ban laws in your jurisdiction before every recruiter screen prep. The result is cached and refreshed every 90 days automatically.

| Source | Coverage |
|---|---|
| State labor department websites | Official state-level salary history and expectation ban laws |
| National Conference of State Legislatures (ncsl.org) | Tracks salary history ban legislation by state, updated regularly |
| U.S. Department of Labor (dol.gov) | Federal wage and hour guidance and pay equity resources |
| EEOC (eeoc.gov) | Pay equity and employment discrimination law guidance |
| Ballotpedia (ballotpedia.org) | Recent legislative changes and ballot measures by state |

Laws change. The skill always directs you to your state Department of Labor to verify current status before an interview.

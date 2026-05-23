# Interview Ready: Claude Skill Package

A five-skill package that takes any job seeker from raw profile to hired. Covers onboarding, resume writing, job search evaluation, full interview pipeline preparation, and live EEOC-compliant mock interview sessions.

Works in Claude.ai (browser), Claude Desktop, and Claude Code. Gemini Gem port planned.

---

## What This Is

Interview Ready is a structured, research-grounded career preparation system built as Claude skills. Each skill is autonomous but context-aware: they share a user profile so you are never asked the same question twice across the workflow.

Every output is grounded in actual company research and your real background. This is not a template generator.

---

## The Five Skills

| Skill | What It Does |
|---|---|
| `skill-01-interviewer` | Onboards the user. Builds the shared profile: background, strengths, weaknesses, job search status and motivation. The starting point for every other skill. |
| `skill-02-resume-writer` | Accepts your resume via file upload or any document source. Rewrites or builds your resume and cover letter in professional, human language. Primary output is Word (.docx). |
| `skill-03-job-search` | Matches your profile to industries and roles. Evaluates job descriptions you bring: individually or in bulk. Tells you whether to apply and how to position. |
| `skill-04-interview-prep` | Prepares you for every stage of the hiring pipeline: recruiter screen, hiring manager, panel, final round, and offer negotiation. Research-grounded, role-specific, candidate-anchored prep documents. |
| `skill-05-mock-interviewer` | Runs a live, interactive mock interview session tailored to a specific job description and the candidate's real background. Three modes: standard practice, deep simulation, and quiz mode. Every answer graded against an EEOC-compliant standard. |

---

## Requirements

- Claude.ai account (Pro, Max, Team, or Enterprise) or Claude Code
- Code Execution enabled: Settings → Capabilities → Code Execution (required for skills to load)
- No API key required. You bring your own Claude environment.

---

## Installation

### Option 1: Claude.ai or Claude Desktop (no technical setup required)

This is the path for most users.

**Step 1: Download the package**

On this GitHub page, click the green **Code** button, then click **Download ZIP**. Save the file to your computer.

**Step 2: Open Claude settings**

- In Claude.ai (browser): go to [claude.ai](https://claude.ai), click your profile icon in the top right, select **Settings**, then click **Features** in the sidebar, then **Skills**.
- In Claude Desktop: open the app, go to **Settings → Capabilities → Skills**.

**Step 3: Enable Code Execution**

In Settings → Capabilities, make sure **Code Execution** is turned on. Skills will not load without it.

**Step 4: Upload each skill**

Each skill is a separate folder inside the ZIP you downloaded. You need to upload them one at a time.

For each of the five skill folders (`skill-01-interviewer`, `skill-02-resume-writer`, `skill-03-job-search`, `skill-04-interview-prep`, `skill-05-mock-interviewer`):

1. Create a new ZIP file containing just that one skill folder
2. Click **Upload skill** in the Skills settings panel
3. Select the ZIP file and upload it
4. Repeat for each skill

Once all four are uploaded, Claude will use them automatically when you start a job search session.

**How to zip a single skill folder (no coding required):**
- On Mac: right-click the skill folder → Compress
- On Windows: right-click the skill folder → Send to → Compressed (zipped) folder

---

### Option 2: Claude Code (command line)

If you use Claude Code, clone the repo and copy the skills to your skills directory:

```bash
git clone https://github.com/ungatedbluemarble/interview-ready.git
cp -r interview-ready/skill-* ~/.claude/skills/
```

Or install per-project (shared with anyone who clones your repo):

```bash
cp -r interview-ready/skill-* .claude/skills/
```

---

### Option 3: Direct GitHub install (Claude Code one-liner)

```bash
mkdir -p ~/.claude/skills && \
  git clone https://github.com/ungatedbluemarble/interview-ready.git /tmp/interview-ready && \
  cp -r /tmp/interview-ready/skill-* ~/.claude/skills/
```

---

## Usage

Once installed, start with Skill 01. Everything else follows from there.

In any Claude session, say:

> "I'm ready to start Interview Ready."

or

> "I need help with my job search."

Skill 01 will onboard you, build your profile, and route you to the right skill based on what you need.

---

## Skill Trigger Reference

You can also go directly to any skill:

| What to say | Skill triggered |
|---|---|
| "Start Interview Ready" / "I need help with my job search" | Skill 01: Interviewer |
| "Write my resume" / "Update my cover letter" | Skill 02: Resume Writer |
| "Find jobs for me" / "Review this job description" | Skill 03: Job Search Expert |
| "I have an interview" / "Prep me for my interview" | Skill 04: Interview Prep |
| "Mock interview" / "Practice with me" / "Interview me" / "Quiz me on the role" | Skill 05: Mock Interviewer |

---

## Bringing In Your Documents

You can feed your resume and other career documents to this package from any source:

- **Direct upload**: drag and drop a file into the Claude chat window
- **Copy and paste**: paste the text of a resume, job description, or any other document directly into chat
- **Cloud storage**: if you have a cloud storage connector enabled in Claude (Google Drive, OneDrive, Dropbox, Box, SharePoint, or any other connected service), you can reference files stored there directly
- **Local files**: in Claude Code or Claude Desktop, you can reference files stored anywhere on your computer
- **Any format**: PDF, Word (.docx), plain text (.txt), or pasted text all work

The skills do not require any specific cloud service. Whatever document source you have access to works.

---

## How Context Works

The user profile built in Skill 01 carries forward into every other skill automatically. Skills pull only the slice of context they need: you are never re-asked for information you already provided.

**Session-persistent (default):** The profile lives in the active conversation. No file is written unless you request it. If you close the session, the profile does not carry over automatically.

**File-based (portable):** Export your profile at any time by saying "save my profile." It saves as `interview_ready_profile.json`. In future sessions, upload that file and any skill reloads your full profile without re-interviewing you.

**Performance note:** Long sessions with heavy document processing: multiple resume revisions, several prep documents: may approach Claude's context limits. If that happens, export your profile, start a new session, upload the file, and continue. The skill will pick up where you left off.

---

## Document Outputs

| Document | Skill | Format |
|---|---|---|
| Resume | Skill 02 | .docx (primary), PDF or plain text on request |
| Cover Letter | Skill 02 | .docx (primary) |
| Recruiter Screen Prep Guide | Skill 04 | .docx |
| Hiring Manager Prep Guide | Skill 04 | .docx |
| Panel Interview Prep Guide | Skill 04 | .docx |
| Final Round Prep Guide | Skill 04 | .docx |
| Offer Negotiation Guide | Skill 04 | .docx |

---

## Disclaimer

Interview Ready is provided for informational purposes only and does not constitute legal, financial, or professional career advice. Compensation data, salary law guidance, and negotiation strategies are research aids, not legal or financial counsel. Use your own judgment and consult a qualified professional where appropriate.

---

## A Note on Salary Law Intelligence

Skill 04 includes a live salary law search that runs on first use and refreshes automatically every 90 days (one US fiscal quarter). When you reach the compensation section of any recruiter screen prep, the skill searches for current salary history and expectation ban laws in your state and city, summarizes what applies to you, and cites the source.

This search is live, not static. The skill does not rely on a fixed list that ages out. It pulls current information, stores the result in your profile, and tells you when it will refresh next.

The result is always accompanied by a disclaimer directing you to your state labor department to verify, because laws change and a web search is not a substitute for official guidance. The skill makes this clear every time.

If you are outside the United States, the skill runs a country and city-specific search instead and flags when reliable information could not be confirmed.

---

## A Note on the Shared Profile Schema

The user profile schema (`user-profile-schema.md`) is intentionally duplicated into the `references/` folder of each skill. This is required because each skill is installed as a separate zip file in Claude and cannot access files outside its own folder. When the schema changes, update all four copies. The `shared/` folder at the repo root is the source of truth , copy from there.

---

## A Note on LinkedIn

LinkedIn blocks automated content fetching. When this package researches a hiring manager, it extracts their name and company from the URL you provide and runs web search queries instead, synthesizing results from public sources: posts, articles, events, and announcements. If minimal public data exists for an interviewer, the skill flags it and focuses on role-level signals instead.

---

## Managing Your Session

Interview Ready is designed to be used across multiple sessions, not in one sitting. Each skill is token-intensive and running the full pipeline end to end in a single conversation will approach Claude's context limits.

**Estimated token usage by stage:**

| Stage | Estimated Tokens |
|---|---|
| Skill 01: Intake and profile build | 2,000 to 4,000 |
| Skill 02: Resume rewrite and cover letter | 8,000 to 15,000 |
| Skill 03: Single JD evaluation | 3,000 to 5,000 |
| Skill 03: Bulk JD review (5 JDs) | 10,000 to 15,000 |
| Skill 04: Single stage prep document | 8,000 to 12,000 |
| Skill 04: Mock interview session | 4,000 to 8,000 |

**Full pipeline estimate:** A complete end-to-end run covering intake, resume, one JD evaluation, recruiter screen prep, hiring manager prep, and a mock interview session consumes roughly 35,000 to 55,000 tokens. Running this in one session is possible but not recommended.

**Recommended approach:**

Run one skill per session. Save your profile at the end of every session by saying "save my profile." Upload the saved file at the start of the next session to pick up exactly where you left off.

Suggested session breakdown:

Session 1: Skill 01 intake and profile build. Save profile.

Session 2: Skill 02 resume and cover letter. Save profile.

Session 3: Skill 03 job search and JD evaluation for your top roles. Save profile.

Session 4: Skill 04 recruiter screen prep for your priority role. Mock interview. Save profile.

Session 5: Skill 04 hiring manager prep when the screen goes well. Save profile.

Continue as needed through panel, final round, and offer negotiation.

**If your session runs out of context:** Say "save my profile" immediately, start a new session, upload the profile file, and continue. The skill will pick up where you left off.

---

## Troubleshooting

**Skills are not triggering**
Make sure Code Execution is enabled in Settings → Capabilities. Skills will not load without it.

**Document upload is not working**
Any file format Claude supports works: PDF, Word, or plain text. If a file fails to upload, paste the text directly into chat instead.

**Session ran out of context**
Say "save my profile" before the session ends. Upload the saved file in a new session to continue without losing your work.

---

## Contributing

Pull requests welcome. If you extend this package for a specific industry, role type, or platform (Gemini, etc.), open a PR with your additions and a note on what changed and why.

---

## License

Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0).

You are free to use, share, and adapt this work for non-commercial purposes as long as you credit @ungatedbluemarble as the original creator and indicate any changes made. Commercial use of any kind is prohibited.

Full license: https://creativecommons.org/licenses/by-nc/4.0/

---

*Built by @ungatedbluemarble. Interview Ready was built from a real workflow and open-sourced because the problem is universal.*

---

## Companion Tools

Interview Ready handles research, documents, and interview preparation. It does not replace a dedicated application tracker. If you are managing more than three active pipelines at once, a visual tracker alongside this package will help you stay organized.

**Application tracking**

These tools are purpose-built for tracking job applications, pipeline stages, contacts, and follow-up dates. All are free to start and require no technical setup.

| Tool | Best For | URL |
|---|---|---|
| Huntr | Dedicated job search tracker with Kanban board, contact management, and resume storage | huntr.com |
| Teal | Job tracker with built-in resume builder and job description analysis | tealhq.com |
| Notion | Flexible tracker if you prefer to build your own board with custom fields | notion.so |
| Airtable | Spreadsheet-style tracker with filtering and views, good for high-volume searches | airtable.com |
| Google Sheets | No-frills option if you want full control and nothing else | sheets.google.com |

None of these are affiliated with or required by this package. They are listed because they solve a problem this package intentionally does not: visual pipeline management across many concurrent applications.

**Using your Interview Ready profile as the source of truth**

Your exported `interview_ready_profile.json` contains an `interview_history` field that tracks every company, role, pipeline stage, and prep session you have run. You can ask Claude at any point:

> "Show me the status of all my active applications."

Claude will read your profile and summarize where you stand across every pipeline you are tracking. This works best when you save your profile after each session so the history stays current.

**Compensation research**

When preparing for offer negotiation, Skill 04 automatically searches these sources and synthesizes them into a base, bonus, equity, and total compensation range for your specific role, level, and location. These sources are also listed so you can cross-reference independently.

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

**Salary law research**

Skill 04 also searches these authoritative sources for salary history and expectation ban laws in your jurisdiction before every recruiter screen prep. The result is cached and refreshed every 90 days automatically.

| Source | Coverage |
|---|---|
| State labor department websites | Official state-level salary history and expectation ban laws |
| National Conference of State Legislatures (ncsl.org) | Tracks salary history ban legislation by state, updated regularly |
| U.S. Department of Labor (dol.gov) | Federal wage and hour guidance and pay equity resources |
| EEOC (eeoc.gov) | Pay equity and employment discrimination law guidance |
| Ballotpedia (ballotpedia.org) | Recent legislative changes and ballot measures by state |

Laws change. The skill always directs you to your state Department of Labor to verify current status before an interview.

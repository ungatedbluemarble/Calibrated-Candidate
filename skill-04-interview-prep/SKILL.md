---
name: interview-ready-interview-prep
description: >
  Prepares a job seeker for every stage of the hiring pipeline from recruiter screen through offer negotiation. Produces research-grounded, candidate-specific prep documents as Word deliverables. Use this skill whenever a user has an interview coming up, asks how to prepare for a recruiter call, wants to know what questions to expect, needs help preparing for a hiring manager conversation, panel interview, or final round, or says "I have an interview", "prep me for my interview", "what should I say", "help me practice", or any variation of interview preparation. Also triggers when the user asks about their application pipeline status, says "where am I with my applications", "show me my pipeline", "what interviews do I have coming up", "add this to my tracker", or any variation of job search status tracking. Requires user profile context from interview-ready-interviewer. If profile is missing, prompt for it before proceeding.
---

# Skill 04: Interview Prep

## Purpose

Prepare the user for every stage of the hiring pipeline with research-grounded, role-specific, candidate-anchored guidance. Every output is built from actual company research and the user's real background: never from generic templates.

---

## Behavior Rules

- Research first, generic never. Every prep document must be grounded in actual data about the company, the role, and where available, the interviewer.
- Anchor discipline. Every talking track must be tied to a specific example from the candidate's background. Abstract claims without anchors are worthless in an interview.
- Be honest about gaps. If the candidate's background does not match a requirement, flag it and advise how to address it: do not paper over it.
- Register awareness. The prep document must match the stage. A recruiter screen and a hiring manager conversation are different conversations requiring different registers. See stage-specific guidance below.
- One document per stage. Do not combine recruiter prep and hiring manager prep into a single document. They serve different purposes.

---

## Pipeline Stages

This skill covers the full hiring pipeline. Confirm which stage the user is preparing for before proceeding.

| Stage | Focus | Output Document |
|---|---|---|
| Recruiter Screen | Competence validation, logistics, compensation | Recruiter Screen Prep Guide |
| Hiring Manager | Strategic fit, thinking style, peer alignment | Hiring Manager Prep Guide |
| Panel Interview | Multi-stakeholder management, consistency | Panel Prep Guide |
| Final Round | Executive presence, vision alignment, closing | Final Round Prep Guide |
| Offer Negotiation | Compensation strategy, total comp, leverage | Offer Negotiation Guide |

---

## Research Phase

Run this before writing any prep document.

### Company Research
- Funding status, valuation, headcount, geography
- Product architecture and primary value proposition
- Recent news, acquisitions, product launches (last 90 days)
- Competitive landscape and primary threats
- Culture signals: Glassdoor, LinkedIn activity, employee reviews
- Financial health signals (for public companies: revenue, growth rate, margin)

### Interviewer Profiling (where applicable)
- Web search: [Name] + [Company] + [Title]
- Extract: career background, tenure at company, public content (posts, reposts, speaking events)
- LinkedIn note: LinkedIn blocks direct fetch. Use name + company in web search and synthesize from results.
- Translate signals into: what they value, how they hire, what register to use in the conversation
- If insufficient public data exists, flag it and focus on role-level signals instead

### Role Analysis
- Parse the job description for explicit requirements, implicit priorities, and language patterns
- Map candidate background to each requirement
- Identify gaps honestly: flag what to acknowledge vs. what to reframe
- Determine the stage register (see below)

---

## Stage-Specific Guidance

### Recruiter Screen

**What it is:** Competence validation and logistics filter. The recruiter is confirming you can do the job and are a reasonable culture fit before passing you to the hiring team.

**Register:** Professional and clear. Demonstrate competence without showing off. Match energy. Answer directly.

**Prep document sections:**
1. Company overview (3-5 bullet facts the candidate should know)
2. Role summary in plain language
3. Likely questions with direct answer guidance
4. Compensation handling (see full guidance below)
5. Questions for the recruiter: what to ask to advance understanding of the role and process
6. Logistics checklist (time, format, who's on the call, next steps to ask for)
7. Draft thank-you note (to send within 24 hours of the call)

**Compensation handling: full guidance**

The salary question in a recruiter screen is the moment most candidates give away negotiating leverage before the process has even started. The prep document must include explicit, word-for-word guidance on how to respond.

The goal is to stay in the conversation without anchoring to a number prematurely. Anchoring too low locks the candidate into a ceiling. Anchoring too high risks screening out before the hiring team is involved. The right move in almost every case is to defer without sounding evasive.

**Salary law search: required before every compensation section.**

Do not rely on static knowledge for salary history and expectation laws. These change as legislation evolves and vary by state and city. Always run a live search before generating compensation guidance.

**Search logic:**

Step 1: Check `salary_law_cache` in the user profile.

Step 2: If `last_searched` is empty (first use) or `next_refresh_due` is earlier than today's date, run a fresh search across the authoritative sources below. If the cache is current, use the stored result and skip the search.

Step 3: Search in this sequence. Check each source in order and stop when a clear, current answer is found. If sources conflict, use the most recent government source as the tie-breaker.

**Salary law authoritative sources (check in order):**

1. State labor department official website: search "[state] department of labor salary history ban" to find the official state page
2. National Conference of State Legislatures: ncsl.org tracks salary history ban legislation by state, updated regularly
3. U.S. Department of Labor: dol.gov for federal wage and hour guidance and pay equity resources
4. EEOC: eeoc.gov for pay equity and employment discrimination law guidance
5. Ballotpedia: ballotpedia.org tracks recent legislative changes and ballot measures by state

Run two queries per jurisdiction:
- "salary history ban law [state] [city if available] [current year]"
- "salary expectation ban [state] [current year]"

These are distinct legal protections and must be checked separately.

Step 4: Extract whether a history ban exists, whether an expectation ban exists, what it covers, effective date, any employer exceptions, and any penalties for non-compliance. Pull the source URL from the most authoritative result found. Never cite a blog, HR aggregator, or law firm marketing page as the primary source.

Step 5: Write the result back to `salary_law_cache` with:
- `last_searched`: today's date in ISO 8601
- `next_refresh_due`: 90 days from today (one US fiscal quarter)
- `source_url`: the primary government or legislative source found
- `summary`: one to two sentence plain-language summary of what applies to this user

Step 6: Surface the result in the prep document with a clear disclaimer:

> "Based on a search conducted today across state labor department records and NCSL legislation tracking, [summary of what applies in their jurisdiction]. Primary source: [source URL]. Laws change. Verify current status with your state Department of Labor before your interview: [state labor dept URL if found]."

**If the search returns no clear result:** Tell the user the skill could not confirm their jurisdiction's status with confidence, direct them to their state labor department website, and include the general guidance below without making a specific legal claim.

**If the user is outside the US:** Note that salary history laws vary significantly by country. Search "salary history disclosure law [country] [city] [current year]" using the same source priority logic where possible. If no reliable official result is found, omit the jurisdiction-specific guidance and advise the user to research their local labor laws independently.

**Compensation benchmarking: required for every offer negotiation prep and recommended for recruiter screen prep.**

Do not rely on static knowledge for compensation ranges. Market rates shift, vary significantly by location and company stage, and differ by data source. Always run a live multi-source search before generating compensation guidance.

**Compensation benchmarking sources (search all, then synthesize):**

Search each of the following for the specific role title, level, and location. Use the exact job title from the job description, not a generic equivalent.

1. Levels.fyi: strongest for verified, self-reported total compensation in technology roles. Search for the exact title and company if available.
2. Glassdoor: broad cross-industry self-reported ranges. Note that Glassdoor data skews toward base salary and may undercount total comp.
3. LinkedIn Salary: tied to real job postings and location-specific data. Useful for understanding what companies are actively paying.
4. Payscale: detailed breakdown by years of experience, education level, and geography. Strong for non-technical roles.
5. Bureau of Labor Statistics (bls.gov): authoritative US government occupational wage data. Use for SOC code-level benchmarks and to anchor the low end of the range.
6. Dice: strongest for technology and engineering roles. Useful for software, data, and infrastructure positions.
7. Built In: strong for startup and growth-stage company compensation. Covers equity and total comp packages at emerging tech companies.
8. Indeed Salary: high volume, broad coverage across industries. Use as a cross-reference.

**Synthesis instructions:**

After searching all sources, produce a compensation summary with:

- Base salary range: low, midpoint, and high based on cross-source data
- Bonus range: typical percentage or flat amount for this role type and level
- Equity range: if applicable, typical grant size and vesting terms for company stage
- Total compensation range: base plus bonus plus annualized equity value
- Geographic adjustment note: if the user's location differs from the primary data market (usually San Francisco or New York), flag the adjustment
- Source agreement note: flag if sources diverge significantly and explain which source is most reliable for this specific role type
- Confidence level: High (3 or more sources in agreement), Moderate (sources diverge), or Low (limited data available)

**Present the benchmarking output in the prep document as:**

> "Based on current market data across [sources checked], the compensation range for [role title] at [level] in [location] is:
> Base salary: [low] to [high], midpoint [midpoint]
> Total compensation (including typical bonus and equity): [low] to [high]
> [Source agreement or divergence note]
> [Geographic adjustment if applicable]
> These figures are research aids, not guarantees. Verify against current postings and use your own judgment in negotiation."

**Anchor guidance:** When the candidate must give a number, advise them to anchor at the 75th percentile of the range, not the midpoint. Candidates consistently underestimate what companies will pay for the right person.

**Check state law first.** Many US states and cities prohibit employers from asking about salary history (what the candidate currently earns or has previously earned). This is different from asking about salary expectations (what the candidate wants). If the user is in a jurisdiction with salary history protections, note this in the prep document so they know they are not required to answer history questions.

The skill's built-in list of covered jurisdictions is a starting point only and may not reflect current law. Always defer to the live search result above.

**Scripts to include in the prep document by scenario:**

When asked "What are your salary expectations?":
> "I am keeping an open mind at this stage. I want to make sure this is the right fit on both sides before we get into specifics. Can you share the budgeted range for this role?"

If the recruiter pushes for a number:
> "I appreciate you asking. I would rather not anchor to a number before I have a full picture of the role, the scope, and the total package. If you can share the range, I can tell you whether we are aligned."

If the recruiter shares a range that is lower than expected:
> "Thank you for sharing that. That is a bit below where I am targeting, but I am interested enough in this role to keep talking. Is there flexibility in the range, or would total compensation including bonus and equity bring it closer to what I have in mind?"

If the recruiter asks about current salary (where permitted):
> "I would prefer to keep that private, but I can tell you that I am targeting [X to Y range] based on my research and the scope of this role."

If the recruiter insists on a number before moving forward and the candidate decides to give one:
> Instruct the candidate to anchor at the top of their target range, not the middle. Candidates consistently underestimate what companies will pay for the right person. Name the number with confidence and do not immediately justify or qualify it.

**What to include in the prep document:**
- The candidate's researched target range for this specific role, location, and company stage (pulled from Skill 04 market research or supplied by the candidate)
- The script most appropriate for their situation
- A reminder that giving a number too early is almost always a mistake
- The jurisdiction note on salary history laws if applicable

### Hiring Manager

**What it is:** Strategic alignment and fit assessment. The hiring manager already knows you can do the job. This conversation is about how you think, whether you'd operate well in their team, and whether you share their vision for the work.

**Register:** Peer-level. Strategic. Specific. Stop selling your resume: start demonstrating your thinking.

**The register shift is critical.** The prep document must explicitly address how the candidate should shift from recruiter-screen mode to hiring-manager mode.

**Prep document sections:**
1. Who the hiring manager is: background, signals, what they value, how they hire
2. What this conversation is actually testing (three modes: strategic fit, team fit, leadership register)
3. The register shift: explicit table showing recruiter screen vs. hiring manager conversation
4. Core narrative: the framing statement the candidate leads with
5. Anticipated questions with anchored answers (grounded in the candidate's specific background)
6. Questions for the interviewer: with subtext on what each one is assessing
7. Things to avoid
8. How to close the conversation
9. Draft thank-you note (to send within 24 hours)

See `/references/hiring-manager-example.md` for the structural example this skill is modeled on.

### Panel Interview

**What it is:** Multi-stakeholder evaluation. Different interviewers are assessing different dimensions. The challenge is consistency while adapting to each person's lens.

**Register:** Varies by panelist. Research each interviewer where possible. Generally: more technical with technical interviewers, more strategic with leaders, more peer-level with peers.

**Prep document sections:**
1. Panel composition: who is on the panel and what they are likely assessing
2. Cross-panelist consistency strategy: the narrative thread that holds across all conversations
3. Per-panelist prep: tailored talking points for each interviewer's lens
4. Anticipated questions by panelist type
5. How to handle conflicting questions or pressure testing
6. Questions for the panel
7. Draft thank-you notes: one per panelist (bracketed fields to complete after the interview)

### Final Round

**What it is:** Executive alignment and closing evaluation. At this stage, fit is largely assumed. The conversation is about vision, leadership presence, and whether the candidate can represent the company externally.

**Register:** Executive. Confident. Forward-looking. Minimal resume references: speak in terms of direction, impact, and strategy.

**Prep document sections:**
1. Executive interviewer profiles
2. Strategic framing: how to position for this level of conversation
3. Vision and direction questions with answer guidance
4. How to demonstrate executive presence without overstating authority
5. Closing the conversation: how to signal genuine commitment without desperation
6. What to do if an offer is coming: how to set up negotiation
7. Draft thank-you note for each executive interviewer

### Offer Negotiation

**What it is:** Compensation and terms negotiation. Most candidates leave money on the table because they do not prepare for this stage.

**Register:** Collaborative, not adversarial. You are aligning on a number that works for both sides.

**Prep document sections:**
1. Market compensation research: base, bonus, equity benchmarks for this role and location
2. Total compensation breakdown: how to evaluate the full package, not just base salary
3. Negotiation strategy: when to push, when to accept, what to ask for beyond salary
4. Scripts: word-for-word language for common negotiation scenarios
5. Walk-away analysis: what is the minimum acceptable offer and how to know when to walk

---

## Document Generation

### Format
- Word document (.docx), US Letter, 1-inch margins, Arial font
- One document per pipeline stage
- Color palette and layout rules: see `/references/docx-style-guide.md`

### Writing style
- Prep documents are written to the candidate, not about them. Use "you" and "your."
- Callout boxes for key statements (core narrative, closing statement, things to avoid).
- Tables for comparisons (register shift, requirement mapping, panel composition).
- No bullets for prose guidance: write in sentences. Bullets only for lists that are genuinely list-like.
- Interview answer guidance is written as an ANCHOR block, not as a script. The candidate adapts it to the conversation. Do not write word-for-word scripts: they make candidates sound rehearsed.

### Async handling
If both a recruiter screen document and a hiring manager document are needed in the same session, generate the recruiter screen first, confirm with the user, then generate the hiring manager version. Do not attempt both in a single pass.

---

## Interview History Tracking

After each prep session, add an entry to the user profile's `interview_history` field:

```json
{
  "company": "",
  "role": "",
  "stage": "",
  "interview_date": "",
  "interviewer_name": "",
  "prep_document_generated": true,
  "outcome": "",
  "notes": ""
}
```

This allows the skill to track where the user is in multiple active pipelines and avoid duplication across sessions.

---

## Handoff

After generating the prep document, ask:
> "Do you want to run a practice session? I can ask you likely questions and give you feedback on your answers."

If yes: enter mock interview mode using the full structure below.

---

## Mock Interview Mode

Mock interview mode is a structured practice session tied to the specific stage and role the user is preparing for. It is not a generic Q&A. Every question asked must be drawn from the prep document already generated for this session.

**Opening the session:**
> "Let's run through some practice questions. I will ask them one at a time, as a real interviewer would. Answer as you would in the actual interview. After each answer I will give you direct feedback on what worked, what to sharpen, and what to cut. Ready?"

Wait for the user to confirm before starting.

**Question selection by stage:**

| Stage | Question focus | Number of questions |
|---|---|---|
| Recruiter Screen | Background, motivation, role fit, compensation | 4-5 |
| Hiring Manager | Strategic thinking, anchored examples, vision alignment | 5-7 |
| Panel | Mix of technical, behavioral, and peer-level questions | 6-8 |
| Final Round | Executive presence, leadership philosophy, forward vision | 4-6 |

Always start with the question the user is most likely to face first in that interview. For a recruiter screen that is almost always "Tell me about yourself" or "Walk me through your background." For a hiring manager it is usually "Why this role?" or "Tell me about yourself" at a higher register.

**Running each question:**

Step 1: Ask the question exactly as a real interviewer would ask it. No preamble, no hints.

Step 2: Wait for the user to answer in full.

Step 3: Give structured feedback using this format every time:

> **What landed:** [What was specific, credible, and compelling. Name the exact phrase or moment.]
>
> **Sharpen this:** [One thing to tighten, clarify, or reframe. Be specific about what to change and why.]
>
> **Cut this:** [Anything that weakened the answer: over-explanation, filler, hedging, or off-topic content. If nothing to cut, say so.]
>
> **Anchor check:** [Did the answer include a specific example grounded in the candidate's real background? If not, name the gap and suggest which example from their profile would fit.]

Step 4: Ask if they want to try the question again before moving on, or proceed to the next question. Do not force a retry. Let the candidate decide.

**Feedback rules:**
- Never say "great answer" without specifying what made it great.
- Never say "that was good but..." The "but" erases everything before it. Use "and" or start fresh with "Sharpen this."
- Never give more than three pieces of feedback per answer. Pick the highest-impact items.
- If the answer was genuinely strong, say so directly and explain why, then move on. Do not manufacture criticism.
- If the answer was weak, be honest. A mock session that lets weak answers pass is useless.

**Tracking weak areas:**
Keep a running internal note of which question types the candidate struggles with. At the end of the session, surface a summary:

> "Here is what I noticed across the session:
> [Strength]: Your strongest answers were on [topic]. You were specific, grounded, and confident.
> [Pattern to address]: You tended to [over-explain / hedge / lose the example / trail off] when answering [type of question]. Before the real interview, spend time on [specific preparation action].
> [One priority]: If you only work on one thing before [interview date], make it [specific answer or topic]."

**Closing the session:**
After the final question and summary, remind the user to save their profile and offer to generate the thank-you note template if they have not already received it.

---

## Profile Save Prompt

After every prep session, whether or not a mock interview is run, prompt the user to save their updated profile:

> "I have added this prep session to your interview history. If you want this saved for future sessions, say 'save my profile' and I will export your updated profile file. Without saving, your interview history will not carry over to a new conversation."

If the user says "save my profile" at any point, write the full current profile object including the updated `interview_history` array to `interview_ready_profile.json` in the output directory and confirm:

> "Your profile has been saved. Upload this file at the start of any future Interview Ready session to pick up where you left off. Your interview history, background, and preferences will all carry forward."

**Why this matters:** Session-persistent context does not survive when the conversation ends. A user managing multiple active job pipelines across several companies and stages will lose all tracking if the profile is not exported. The prompt must appear at the end of every prep session without exception, even if the user did not ask for it.

---

## Pipeline Status Dashboard

Trigger this when the user asks any of the following:
- "Where am I with my applications?"
- "Show me my pipeline status"
- "What interviews do I have coming up?"
- "Which companies am I tracking?"
- "Give me a summary of my job search"
- Any variation of requesting an overview of their active applications

**How to generate the dashboard:**

Read the `interview_history` array from the current profile. For each entry, determine the current status using this logic:

| outcome field value | Status label |
|---|---|
| "" (empty) | Active |
| "scheduled" | Interview Scheduled |
| "prep complete" | Prep Complete |
| "awaiting feedback" | Awaiting Feedback |
| "offer received" | Offer Received |
| "offer accepted" | Accepted |
| "offer declined" | Declined |
| "rejected" | Closed: Rejected |
| "withdrawn" | Closed: Withdrawn |
| "no response" | Closed: No Response |

**Dashboard output format:**

Present the dashboard as a clean summary table followed by a brief narrative on what needs attention. Do not dump raw JSON at the user.

Example output structure:

---
**Your Job Search Pipeline**

| Company | Role | Stage | Interview Date | Status |
|---|---|---|---|---|
| [Company] | [Role] | Hiring Manager | June 4 | Prep Complete |
| [Company] | [Role] | Recruiter Screen | June 7 | Interview Scheduled |
| [Company] | [Role] | Panel | TBD | Awaiting Feedback |

**What needs attention:**
- Your [Company] panel was [X days] ago with no outcome recorded. Do you want to update the status or prep a follow-up?
- Your [Company] recruiter screen is in [X days]. Do you want to run prep now?
- You have not logged any activity on [Company] in [X days]. Do you want to mark it closed or follow up?

---

**Prompts to offer after the dashboard:**
- "Do you want to prep for any of these upcoming interviews?"
- "Do you want to update the status on any of these?"
- "Do you want to add a new application to track?"

**Updating a record:** If the user wants to update the outcome or notes on an existing entry, update the relevant fields in the `interview_history` array and prompt them to save their profile.

**Adding a new application without running full prep:** The user can say "add [Company] [Role] to my tracker" and Skill 04 will create a new history entry with the company, role, and stage populated, outcome left empty, and remind them to save their profile.

**If interview_history is empty:** Tell the user their tracker is empty and offer two paths: run prep for a role they are currently pursuing, or manually add applications they are already tracking.

---

## Thank-You Notes

A well-written thank-you note sent within 24 hours of an interview is one of the lowest-effort, highest-impact actions a candidate can take. It is standard professional practice. Skipping it is noticed more often than candidates expect, particularly at the hiring manager and final round stages.

**When to send:** Within 24 hours of every interview at every stage. For panel interviews, send a separate note to each panelist if you have their contact information.

**How to get contact information:** The recruiter is the easiest path. Before or after the screen, ask: "Could you share the email addresses of the people I will be meeting with so I can follow up appropriately?" Most recruiters will provide them without hesitation.

**Generate a thank-you note as part of every prep document.** Do not wait for the user to ask. After generating the prep document for any pipeline stage, include a draft thank-you note at the end, ready to send after the interview.

**Format and length:** Email. Four to six sentences. No more. A thank-you note that runs longer than one short paragraph reads as overcompensating.

**Structure:**

Line 1: Thank them by name for their time and the specific conversation. Reference something specific that was discussed, not a generic "it was great to meet you." This is what separates a real note from a template.

Line 2-3: One sentence reinforcing your fit for the role, tied to something that came up in the conversation. This is not a second cover letter. It is a single, confident restatement.

Line 4: Express genuine interest in moving forward. Do not grovel. Do not say "I would be honored." Say "I am looking forward to next steps."

Line 5: Close cleanly. "Thank you again for your time" is sufficient. Sign with your full name.

**What NOT to do:**
- Do not send the same note to every interviewer on a panel. Each note must reference something specific to that conversation.
- Do not use "I wanted to reach out to thank you." Just thank them. Remove the preamble.
- Do not restate your entire background. One sentence on fit is enough.
- Do not send via LinkedIn unless email is not available. Email is the professional default.
- Do not follow up on the thank-you note if you do not hear back. One note is the standard. Two looks anxious.

**Draft thank-you note template for prep documents:**

> Subject: Thank you, [First Name]
>
> [First Name],
>
> Thank you for taking the time to speak with me today about the [Role] position at [Company]. I particularly appreciated your perspective on [specific topic discussed]: it gave me a clearer picture of [what the role requires / how the team operates / what success looks like].
>
> The conversation reinforced my interest in the role. [One sentence connecting a specific point from the conversation to the candidate's background or a relevant accomplishment.]
>
> I am looking forward to next steps and the opportunity to continue the conversation.
>
> Thank you again,
> [Full Name]

**Personalization is required.** The bracketed fields are not optional. A thank-you note that could have been sent to anyone provides no value. Claude must ask the user what was discussed before drafting the note, or note in the prep document that the bracketed sections must be completed after the interview.

**For panel interviews:** Generate one template per panelist with a note reminding the user to personalize each one based on what they discussed with that specific person.

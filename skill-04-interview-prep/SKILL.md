---
name: interview-ready-interview-prep
description: >
  Prepares a job seeker for every stage of the hiring pipeline from recruiter screen through offer negotiation. Produces research-grounded, candidate-specific prep documents as Word deliverables. Use this skill whenever a user has an interview coming up, asks how to prepare for a recruiter call, wants to know what questions to expect, needs help preparing for a hiring manager conversation, panel interview, or final round, or says "I have an interview", "prep me for my interview", "what should I say", "help me practice", or any variation of interview preparation. Requires user profile context from interview-ready-interviewer. If profile is missing, prompt for it before proceeding.
---

# Skill 04: Interview Prep

## Purpose

Prepare the user for every stage of the hiring pipeline with research-grounded, role-specific, candidate-anchored guidance. Every output is built from actual company research and the user's real background: never from generic templates.

---

## Behavior Rules

- Research first, generic never. Every prep document must be grounded in actual data about the company, the role, and where available, the interviewer.
- Never use em dashes (U+2014) or en dashes (U+2013) in any output generated for the user. Use commas, colons, or restructure the sentence. This applies to all documents, inline responses, HTML files, and any other content produced in this session.
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

**HTML compensation report (on request only):** After delivering the compensation section of any prep document, offer once:
> "Would you like an HTML version of this compensation analysis formatted for easy reference during your negotiation?"

Only generate if the user says yes. Use the Interview Ready design system (DM Serif Display, DM Sans, DM Mono, CSS variables matching the site). Include salary law findings, the benchmarking table, a visual range bar with the recommended anchor marked, and the negotiation scripts. Do not generate this automatically.

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

### Reference Preparation

When a candidate reaches the final round or post-offer stage, advise them to prepare their references proactively. Do not wait for the user to ask. See `/references/reference-preparation.md` for full guidance on who to choose, how to ask, how to brief references, and what reference checkers typically ask.

Surface reference preparation as a prep action item in the Final Round prep document and in the Offer Negotiation guide.

### Follow-Up Cadence

After generating any stage prep document, include a follow-up cadence note telling the candidate exactly when and how to follow up if they do not hear back. See `/references/follow-up-cadence.md` for stage-by-stage timing, email templates, and guidance on when to stop following up.

Do not leave the candidate without explicit follow-up instructions after any prep document is generated.

### Resignation Handling

When a candidate receives and accepts an offer, provide resignation handling guidance before closing the session. See `/references/resignation-handling.md` for guidance on timing, the resignation conversation, notice period, counteroffer handling, and the transition period.

Surface this guidance proactively in the Offer Negotiation guide under a section titled "After You Accept."

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
  "recruiter_email": "",
  "prep_document_generated": true,
  "outcome": "",
  "notes": ""
}
```

This allows the skill to track where the user is in multiple active pipelines and avoid duplication across sessions.

**Recruiter email collection:** When generating any stage prep document, ask the user for the recruiter's email address if it is not already in the profile entry for this role. Frame it as follows:

> "Do you have the recruiter's email address for this role? If you share it, I can check your inbox for any responses when you view your pipeline status."

If they provide it, write it to `recruiter_email` in the corresponding `interview_history` entry and prompt them to save their profile. If they do not have it yet, leave the field empty. Do not block prep document generation on this: it is supplementary context, not a requirement.

---

## Handoff

After generating the prep document, ask:
> "Do you want to run a practice session? I can ask you likely questions and give you feedback on your answers."

If yes: do not run mock interview logic inside this skill. Route to Skill 05: Mock Interviewer, which handles all practice sessions including standard practice, deep simulation, and quiz mode. Pass the current role, stage, and prep document context so Skill 05 can calibrate its questions to this specific interview.

---

## Profile Save Prompt

After every prep session, whether or not a mock interview is run, prompt the user to save their updated profile:

> "I have added this prep session to your interview history. If you want this saved for future sessions, say 'save my profile' and I will export your updated profile file. Without saving, your interview history will not carry over to a new conversation."

If the user says "save my profile" at any point, write the full current profile object including the updated `interview_history` array to `interview_ready_profile.json` in the output directory and confirm:

> "Your profile has been saved. Upload this file at the start of any future Interview Ready session to pick up where you left off. Your interview history, background, and preferences will all carry forward."

**Why this matters:** Session-persistent context does not survive when the conversation ends. A user managing multiple active job pipelines across several companies and stages will lose all tracking if the profile is not exported. The prompt must appear at the end of every prep session without exception, even if the user did not ask for it.

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

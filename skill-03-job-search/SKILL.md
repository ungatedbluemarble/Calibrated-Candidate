---
name: calibrated-candidate-job-search
description: >
  Matches a job seeker's profile to industries and roles, evaluates job descriptions, and provides actionable guidance on how to pursue specific opportunities. Use this skill whenever a user wants to find jobs that match their background, asks whether they are a fit for a specific role, pastes or shares a job description for review, references a folder of job postings from any connected cloud storage or local source, or asks "what jobs should I be applying for", "does this role fit me", "review this job posting", "evaluate this JD", "am I a good fit for this", or any variation of job search evaluation or job description review. Trigger even when the user pastes a block of text that resembles a job posting without explicitly requesting a review. Requires user profile context from calibrated-candidate-interviewer. If profile is missing, prompt for it before proceeding.
---

# Skill 03: Job Search Expert

## Purpose

Match the user's background to the right industries and roles. Evaluate job descriptions against the user's profile. Provide honest, direct guidance on fit, gaps, and how to proceed: including whether to apply, how to position, and what to address.

---

## Behavior Rules

- Be honest about fit. If a role is a stretch, say so and explain why. If it's a strong match, say so with specificity.
- Never use em dashes (U+2014) or en dashes (U+2013) in any output generated for the user. Use commas, colons, or restructure the sentence. This applies to all documents, inline responses, HTML files, and any other content produced in this session.
- Never encourage a user to apply for a role they are not qualified for without flagging the gaps clearly.
- Do not over-qualify candidates out of roles they can do. Conservative framing that undersells a candidate is as harmful as overselling.
- When evaluating a JD, distinguish between hard requirements (must-have) and preferred qualifications (nice-to-have). Many candidates self-reject on preferred items: surface this distinction.
- Never present a role found through proactive search to the user without first verifying it is open, remote-eligible (if applicable), and has a working application path. This applies to every role the skill surfaces, not just roles the user brings in.
- When a proactive role search returns fewer confirmed open roles than requested, do not pad the list or tell the user to search on their own. Instead run the No Results Protocol before ending the search.

---

## Industry and Role Matching

Run this when the user does not have a specific job in mind and needs direction.

### Step 1: Profile Review
Pull from the user profile (Skill 01):
- `background.functional_domain`
- `background.years_of_experience`
- `search_status.target_industries`
- `search_status.target_roles`
- `strengths`
- `experience` (titles and domains)

### Step 2: Industry Fit Analysis
Based on the profile, identify three to five industries where the user's background transfers well. For each:
- State the industry
- Explain why the user's experience maps to it
- Name two to three representative job titles to target
- Flag any credentialing or knowledge gaps to address

### Step 3: Role Targeting
Recommend five to eight specific job titles the user should be searching for. For each title:
- Why it fits
- What variations of the title to search (titles vary widely by company and sector)
- What level (IC, manager, director, VP) they should be targeting based on experience

Present findings in a structured format the user can act on immediately.

### Step 4: Proactive Role Validation

When specific open roles are identified and surfaced to the user, each role must pass three checks before it is presented.

**Check 1: Confirm the role is open.** Fetch the direct job posting URL. A rendered job description with a visible apply button is NOT by itself proof that a role is open. Applicant tracking systems (Workday, Greenhouse, Lever, iCIMS) and aggregators (Built In, Jobright, LinkedIn, Indeed, ZipRecruiter, RemoteRocketship) routinely keep serving the full description and a non-functional apply control after the requisition has closed. Treat the body of a posting as untrusted for open/closed status. Confirm open status by running all three steps below before presenting any role.

1. Scan the entire page for closed signals, not just the job-description body. Closed signals include, in any casing or location on the page: "this job has closed," "no longer accepting applications," "this position has been filled," "this job was removed," "applications are closed," "position closed," "no longer available," a greyed-out or absent apply control, or a redirect to a general careers landing page. A single closed signal anywhere on the page means the role is closed, even if the job-description body still reads as active and an apply button is still visible.

2. Apply the age heuristic. Find the posting date or "posted N days/months ago" indicator. In AI enablement and comparable fast-moving categories, a posting older than roughly 45 days is presumed closed unless an explicit open indicator (active dated application window, "still accepting applications," a confirmed-working apply form) proves otherwise. State the posting age to yourself before scoring the role.

3. Rank source trust. The company's own ATS or careers page is the highest-trust source for open status. Aggregators are lower trust and frequently stale. When possible, confirm open status on the company's own ATS rather than on an aggregator. If only an aggregator can be reached, lower your confidence accordingly and say so in the confirmation line (Check 4).

**Conflict rule: closed always wins.** If any source or any element of a page indicates the role is closed while another indicates it is open, treat the role as closed. Do not present a role on the strength of an active-looking JD body when a closed flag is present elsewhere. Remove closed roles and find a replacement. Do not disclose closed roles to the user.

**Check 2: Confirm remote eligibility.** Extract the location and remote status from the posting text. If the role requires on-site or hybrid attendance inconsistent with the user's location constraints, remove it. If remote status is ambiguous, include a note: "Remote eligibility not confirmed. Verify before applying." Never describe a role as remote unless the posting explicitly states it.

**Check 3: Confirm the application path.** Extract the direct application URL from the posting. If it resolves to an active form, present it. If only a general careers page can be confirmed, tell the user: "Apply via [Company] careers site by searching [Role Title]." Do not fabricate direct links.

**Check 4: Disclose what was actually confirmed.** Validation that depends on fetching live pages is fragile: pages cache, redirect, block automated access, and go stale. Do not present validation as more certain than it is. For every role surfaced, include a one-line confirmation stating what was verified and how, so the user can judge the freshness for themselves. Use this format:

> Verified: [open status: confirmed open via active apply form on company ATS / open status not independently confirmed, aggregator only] | [remote status] | [posting age] | checked [date].

If open status could not be confirmed on the company's own ATS, say so plainly in this line rather than implying certainty. A role whose open status rests only on an aggregator must carry the words "not independently confirmed" so the user verifies before investing time.

If fewer roles pass validation than the target number, run the No Results Protocol below before presenting results to the user.

---

## No Results Protocol

Run this whenever a proactive role search fails to surface the minimum number of confirmed open roles that match the user's stated criteria. Do not tell the user to search on their own. Do not end the session. Do not pad the list with unverified roles. Follow these steps in order.

### Step 1: Disclose what was found

Tell the user honestly and briefly: "I confirmed [N] active roles that match your criteria. I could not verify enough to give you a full list."

If N is zero, say: "I was not able to confirm any currently open roles that match all of your criteria."

Do not explain the search mechanics or apologize at length. One sentence is enough.

### Step 2: Diagnose the constraint

Identify which criteria are most likely narrowing the field. Common culprits in order of likelihood:

- Compensation floor is above market rate for the role type and seniority level
- Remote requirement eliminates most available roles in this category
- IC-only constraint eliminates roles that blend IC and people leadership
- Title or function is too narrowly defined and equivalent roles use different titles
- Industry target is too specific

Do not guess. Pull from what the search actually returned: if roles appeared but were above the comp floor, say so. If roles appeared but required on-site, say so.

### Step 3: Ask one targeted question

Ask the user a single question that addresses the most likely constraint. Frame it as a choice, not an open-ended question. Examples:

If comp is the likely constraint:
> "The roles I found in this space are mostly ranging from [X] to [Y]. Your current floor is [Z]. Would you like me to search at [lower floor] to see what opens up, or keep the floor and I will keep looking?"

If remote is the likely constraint:
> "Most confirmed open roles in this category are hybrid rather than fully remote. Would you like me to include hybrid roles with [N] days in-office, or keep the search remote-only?"

If IC-only is the likely constraint:
> "Several roles I found blend IC work with light team oversight. Would you like me to include roles where people leadership is a minor component, or keep the search to pure IC?"

If title is too narrow:
> "This exact title is not posting frequently right now. Would you like me to search under related titles like [X], [Y], or [Z]?"

### Step 4: Wait for the user's answer

Do not assume. Do not run a new search until the user responds. When the user adjusts a constraint, run a new validated search immediately using the updated parameters.

### Step 5: If all constraints are adjusted and results are still insufficient

After two rounds of constraint adjustment with no confirmed results, tell the user:
> "The market for this combination of role, comp, and location appears limited right now. I can set up a watch for new postings and check again in [timeframe], or we can shift strategy to direct outreach at companies where you have existing relationships. Which would you prefer?"

Then route based on their answer. Do not abandon the user or tell them the search is complete when it is not.

---

## Job Description Evaluation

Run this when the user provides a specific JD: via paste, URL, or cloud folder.

### JD Input Methods

**Direct paste:** User pastes the job description text into chat. Process immediately.

**URL:** Fetch the page content and extract the job description. If the page blocks fetching, ask the user to paste the text.

After fetching, validate that the role is actively accepting applications before proceeding. An active role has a functional apply button, an apply link, or an embedded application form. A closed role returns a page with the job description present but no apply mechanism, or a message indicating the role has been filled or removed.

If the role is closed: do not evaluate it. Surface one line to the user: "The [Job Title] role at [Company] is no longer accepting applications. Skipping. Want me to search for similar active roles?" Then stop and wait for the user's response.

If the role is active: proceed with the evaluation framework.

If the apply status is ambiguous and cannot be determined from the page: proceed with the evaluation and note at the top of the output: "Note: active application status could not be confirmed for this role. Verify the apply link before submitting."

**Cloud folder or local source:** If a cloud storage connector is active in the session (Google Drive, OneDrive, Dropbox, Box, SharePoint, or any other connected service), access the referenced folder and list the files. Ask the user which to evaluate, or if they request bulk review, process all files and return a ranked summary. If no connector is active, ask the user to upload files directly or paste the job description text into chat. Never tell the user a specific service is required: any connected source works.

### Evaluation Framework

For each job description, produce a structured assessment:

**1. Role Summary**
What this job actually is in plain language. One to two sentences.

**2. Fit Score**
Rate overall fit: Strong / Moderate / Stretch. One sentence explaining the rating.

**3. Requirement Mapping**
Two-column table:

| Requirement | Candidate Status |
|---|---|
| [Requirement from JD] | Met / Partial / Gap |

Distinguish hard requirements from preferred. Candidates frequently self-reject on preferred items: flag this explicitly.

**4. Strengths in This Context**
Which parts of the candidate's background are directly relevant and compelling for this specific role.

**5. Gaps to Address**
Honest assessment of what the candidate does not have. For each gap:
- Is it a likely disqualifier or a manageable concern?
- How should the candidate address it (cover letter framing, interview answer, or honest acknowledgment)?

**6. Application Recommendation**
Apply / Apply with positioning / Do not apply: with a one-sentence rationale.

**7. If Applying: How to Position**
What angle the candidate should lead with for this specific role. What to emphasize in the cover letter and what to prepare for in the screen.

---

## Bulk JD Review

When the user provides multiple job descriptions (folder or batch):

1. Validate each role using the full four-check process in Step 4: Proactive Role Validation above, including the closed-signal scan, the age heuristic, the source-trust ranking, and the closed-always-wins conflict rule. A rendered JD with a visible apply button is not proof the role is open.
2. Skip any role that is closed. Include a single line in the summary output for each skipped role: "[Job Title] at [Company]: no longer accepting applications. Skipped."
3. Process only active roles through the evaluation framework.
4. Return a ranked summary table of active roles only: Role | Company | Fit Score | Key Strength | Key Gap | Recommendation
5. Recommend the top two to three to prioritize
6. Offer to go deep on any specific one

Do not produce full evaluations for every JD in bulk mode: return the summary first and drill on request. Closed roles do not appear in the ranked table.

---

## Web Research

When the user is evaluating a specific company or role and needs market context:

- Research the company: funding stage, headcount, product, recent news, leadership
- Research the role: typical compensation range, common requirements in the market, how the title varies across companies
- Surface any red flags: recent layoffs, negative Glassdoor patterns, legal or financial issues
- Cite sources. Do not present unsourced claims as fact.

---

## Handoff

After JD evaluation, ask:
> "Would you like me to tailor your resume and cover letter for this role, move to interview prep, or run a mock interview session for this role?"

Route to Skill 02, Skill 04, or Skill 05 based on the answer.

If the user has identified specific companies or roles they want to pursue but has not yet applied or made contact, also offer:
> "Would you like help with a networking outreach message? A well-written note to a connection at the company or a cold outreach to a hiring manager can open doors that the application alone will not."

If the user says yes, proceed using Claude's native capability. Write the outreach message based on the specific company, role, and the user's background from the profile. Keep it concise: three to four sentences maximum. Lead with a genuine connection point or reason for reaching out, not a generic compliment. State clearly what the user is looking for and why this company specifically. End with a low-friction ask, not a demand for a meeting.

---

## Career Changer Handling

A career changer is any candidate making a meaningful shift in industry, function, or both. This includes but is not limited to: leaving a specific industry for a different one, moving from an individual contributor role to management or vice versa, transitioning from a technical to a non-technical function, returning from military service to civilian employment, pivoting after a period of entrepreneurship, or re-entering the workforce in a different capacity than before.

Career changers require a different analytical frame than candidates staying in their current lane. The skill must detect this situation and adjust accordingly.

**Detecting a career change:**
Compare `background.industry` and `background.functional_domain` in the user profile against the target industries and roles in `search_status.target_industries` and `search_status.target_roles`. If there is a meaningful mismatch, flag it and apply career changer logic.

Also detect from the user's own language during intake: phrases like "I want to get out of," "I am trying to move into," "I have always wanted to work in," or "I spent time in [field] but now want to" are clear signals.

**Do not treat a career change as a problem.** Frame it as a transition that requires deliberate positioning, not a liability that requires explanation or apology.

---

**Transferable skills analysis:**

When evaluating a career changer against a job description, the standard requirement mapping table is insufficient on its own. Add a second analysis layer:

| Transferable Skill | Source (where they demonstrated it) | Target (where it applies in the new role) |
|---|---|---|
| [Skill from prior career] | [Specific role or context] | [How it maps to the target function] |

The goal is to make the translation explicit rather than leaving it to the hiring manager to figure out. Hiring managers frequently pass on career changers not because the candidate lacks the skill, but because the connection was never made clearly.

---

**Role targeting adjustments for career changers:**

Career changers often need to accept a level adjustment when entering a new field. A director-level candidate in one industry may need to target senior manager or manager roles in a new one, depending on how much domain knowledge the new role requires.

When advising on role targeting, surface this directly:

> "Based on your background, you are targeting [level] roles in [new field]. Given that this is a new industry for you, I want to flag that you may get stronger traction targeting [one level down] roles initially. Hiring managers in [new field] often want to see domain experience before promoting to [level]. Moving in at [one level down] with a strong first year is a faster path to [target level] than struggling to land [target level] directly. That said, it depends on how transferable your specific background is. Let me assess."

Then proceed with the JD evaluation using the transferable skills frame.

---

**Resume adjustments for career changers (coordination with Skill 02):**

Flag to Skill 02 when the candidate is a career changer so the resume is written with transfer framing, not a straight recitation of prior experience in the old field.

Key adjustments Skill 02 must make for career changers:

The Professional Summary must lead with the transferable value, not the prior industry. A candidate moving from military logistics to supply chain operations should not open with "15-year military veteran." They should open with "Operations and logistics leader with 15 years of experience managing complex, high-stakes supply chains under pressure."

Experience bullets must be written in language that maps to the target industry. Military jargon, legal terminology, academic language, and other field-specific vocabulary must be translated into the register of the target field without losing the substance of what was accomplished.

The Signature Projects section is especially valuable for career changers because it lets them lead with outcomes that are field-agnostic before the reader reaches the experience section and notices the industry mismatch.

---

**Cover letter adjustments for career changers:**

The cover letter must directly address the transition. Candidates who do not address it leave the hiring manager to draw their own conclusions, which is almost always worse than a confident, direct explanation.

The career change should be addressed in one paragraph, no more, and framed as intentional rather than reactive. The framing should answer three questions in sequence:

1. Why are you making this move?
2. Why now?
3. What makes you suited for it despite the non-traditional path?

What NOT to do: Do not apologize for the transition. Do not over-explain it. Do not spend more than one paragraph on it. Make the case and move on.

---

**Interview prep adjustments for career changers (coordination with Skill 04):**

Flag to Skill 04 when the candidate is a career changer. Skill 04 must include a dedicated section in the prep document on handling the "Why are you making this change?" question, which will appear in every stage of the pipeline without exception.

The answer must be:
- Confident and forward-looking, not apologetic or defensive
- Grounded in something specific about the target field or role, not just dissatisfaction with the current one
- Brief: two to three sentences maximum before pivoting to what the candidate brings to the new field

Weak answer pattern to coach against: "I have always been interested in [new field] and I feel like my skills could transfer." This is vague and passive. Every career changer says something like this.

Strong answer pattern to coach toward: "I have spent [X years] in [prior field] building [specific skill set]. What I kept running into was [specific problem or pull toward the new field]. This role is specifically where [prior experience] and [target field need] intersect, and I want to be where that work is happening."

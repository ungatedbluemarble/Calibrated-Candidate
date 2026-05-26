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

---

## Job Description Evaluation

Run this when the user provides a specific JD: via paste, URL, or cloud folder.

### JD Input Methods

**Direct paste:** User pastes the job description text into chat. Process immediately.

**URL:** Fetch the page content and extract the job description. If the page blocks fetching, ask the user to paste the text.

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

1. Process each JD through the evaluation framework
2. Return a ranked summary table: Role | Company | Fit Score | Key Strength | Key Gap | Recommendation
3. Recommend the top two to three to prioritize
4. Offer to go deep on any specific one

Do not produce full evaluations for every JD in bulk mode: return the summary first and drill on request.

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

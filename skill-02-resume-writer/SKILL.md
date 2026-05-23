---
name: interview-ready-resume-writer
description: >
  Writes or rewrites a professional resume and cover letter for any job seeker. Use this skill whenever a user asks to update their resume, write a cover letter, improve their resume, tailor their resume to a job, or produce a Word document for a job application. Trigger on: "write my resume", "rewrite my resume", "update my resume", "cover letter", "help me apply", or any request to produce a job application document. Requires user profile context from interview-ready-interviewer. If profile is missing, prompt for it before proceeding.
---

# Skill 02: Resume Writer

## Purpose

Produce a polished, professional resume and cover letter that accurately represents the user's experience in clear, direct, human language. Output is a Word document (.docx) as primary deliverable. PDF and plain text are available on request.

---

## Writing Standards

These are non-negotiable for every document produced by this skill.

**Voice and tone**
- Professional, direct, and human. Not corporate filler.
- No em dashes. Use commas, periods, or restructure the sentence.
- No buzzwords: passionate, driven, results-oriented, synergy, leverage (as a verb), utilize.
- Write as if a sharp, confident professional wrote it themselves: not as if a template generated it.

**Experience bullets**
- Lead with an action verb, past tense for past roles, present tense for current.
- Quantify wherever the data exists. If the user did not provide numbers, ask before fabricating.
- One idea per bullet. No run-on bullets.
- Maximum three to five bullets per role. Quality over volume.
- Format: [Action verb] + [what you did] + [result or scale where available]

**Cover letter**
- Written in paragraph form. No bullets in the body.
- Four to five paragraphs: opening hook, two body paragraphs, optional third body, close.
- Opening must not start with "I am writing to apply for." Reference the role specifically and lead with relevance.
- Must read as written by a person, not assembled from parts.
- No generic filler. Every sentence earns its place.

See `/references/writing-examples.md` for structural examples derived from approved documents.

---

## Document Inputs

The skill accepts resume content from three sources, in order of preference:

1. **Uploaded file**: User uploads their current resume (PDF, DOCX, or TXT). Parse and extract all content. Confirm extracted data before proceeding.

2. **Cloud-connected storage**: User references a file in Google Drive, OneDrive, or Box. If a connector is active in the session, fetch the file. If not, prompt the user to upload directly or enable the connector.

3. **User profile**: If no document is available, use the experience captured in the profile from Skill 01. Flag to the user that the resume will be built from their intake answers and ask them to fill any gaps.

---

## Async Document Handling

Document parsing is token-heavy. To prevent blocking the session:

1. Acknowledge receipt immediately: "I have your resume. I'm processing it now: feel free to tell me about the role you're targeting while I work."
2. Parse in the background while the user provides additional context (target role, JD, tone preferences).
3. Confirm extracted content with the user before writing begins.
4. If parsing fails or produces incomplete output, surface the gaps and ask the user to fill them conversationally.

Do not hold the conversation hostage to document processing.

---

## Resume Structure

Follow this structure exactly. Do not add sections not listed here unless the user explicitly requests them.

```
[FULL NAME]
[City, State]  •  [email]  •  [LinkedIn URL]

PROFESSIONAL SUMMARY
2-3 sentences. Lead with functional identity and years of experience.
Close with the type of work they do and what it produces.
No fluff. No generic claims.

CORE COMPETENCIES
Two rows, three columns each. Functional groupings, not skill dumps.
Format: Category: Item 1 • Item 2 • Item 3

[SIGNATURE PROJECTS or KEY ACHIEVEMENTS: optional]
Use only if the user has 2+ high-impact, quantifiable accomplishments
that don't fit cleanly into a single role.
Each project gets: name, one-line description, two to three bullets.

PROFESSIONAL EXPERIENCE
[Title]  |  [Employer]  [Start Date] to [End Date or Present]
• Bullet
• Bullet
• Bullet

[Repeat for each role, most recent first]

CERTIFICATIONS AND ADDITIONAL
[Certifications listed as: Name (Issuer)]
[Other relevant notes: military service, board positions, founder roles]

EDUCATION
[Degree, Field: Institution, Year]
```

See `/references/resume-structure-example.md` for a complete annotated example.

---

## Cover Letter Structure

```
[FULL NAME]
[City, State]  •  [email]  •  [LinkedIn URL]

[Date]

[Hiring contact or "Hiring Team, [Department]"]
[Company Name]

[Opening paragraph: 3 to 4 sentences]
Lead with the role name and a direct statement of relevance.
Not "I am writing to apply." Open with the connection between
their work and the candidate's experience.

[Body paragraph 1: 4 to 6 sentences]
Most relevant experience cluster. One specific accomplishment with
numbers if available. Connect it explicitly to what the role requires.

[Body paragraph 2: 4 to 6 sentences]
Second experience cluster or cross-functional strength.
Demonstrate breadth without losing specificity.

[Optional body paragraph 3]
Governance, compliance, leadership, or any dimension the role
specifically calls for that hasn't been addressed yet.

[Closing paragraph: 3 to 4 sentences]
Restate fit without restating the resume.
Express genuine interest in a conversation.
Close with a confident, non-groveling sign-off.

Regards,

[Full Name]
```

---

## Tailoring to a Job Description

If the user provides a job description:
1. Parse the JD for explicit requirements, implicit priorities, and language patterns.
2. Mirror the JD's language in the resume and cover letter where it accurately reflects the candidate's experience. Do not fabricate alignment.
3. Flag any requirements the candidate does not meet. Do not paper over gaps: surface them and advise how to address them in the cover letter or interview.
4. Adjust the Professional Summary to lead with the specific value proposition for this role.

---

## Output Format

**Primary:** Word document (.docx)
- US Letter, 1-inch margins, Arial font
- Color palette and layout rules: see `/references/docx-style-guide.md`
- Validate with docx validation script after generation

**On request:** PDF, plain text (ATS-safe, no formatting)

Inform the user:
> "Your resume is ready as a Word document. Say 'give me the PDF version' or 'ATS plain text' if you need either of those formats."

**HTML before and after (on request only):** After delivering the Word document, offer once:
> "Would you like an HTML version showing the before and after of your rewrite, formatted for sharing or presenting visually?"

Only generate if the user says yes. Use the Interview Ready design system (DM Serif Display, DM Sans, DM Mono, CSS variables matching the site). Show the original language on the left and the rewritten language on the right. Anonymize with Jane Doe or John Doe if the user requests it for sharing. Do not generate this automatically.

---

## Performance Notes

Document generation is token-intensive. If both resume and cover letter are requested together, generate the resume first, confirm with the user, then generate the cover letter. Do not attempt both in a single pass: it risks context overflow and quality degradation on long experience histories.

For users with 15+ years of experience, flag that the experience section may need to be trimmed to the most recent 10 to 12 years for readability. Ask before trimming.

---

## Employment Gap Handling

Employment gaps are common and carry less stigma than candidates fear. The skill's job is to help the user present their gap honestly and confidently, not to hide it.

**Detect gaps** by reviewing the experience timeline in the user profile or uploaded resume. A gap is any period of six months or longer between roles, or between the most recent role end date and the present.

**Ask before writing.** When a gap is detected, ask the user directly:
> "I notice a gap in your timeline from [date] to [date]. Can you tell me what you were doing during that period? This helps me present it accurately."

Do not assume the reason. Do not fill the gap with vague language before asking.

**Framing by gap type.** Once the reason is known, apply the appropriate treatment:

| Gap Type | Resume Treatment | Cover Letter Treatment |
|---|---|---|
| Caregiving (family, child, elder) | List as "Career break: family caregiving" with date range. No elaboration needed on the resume. | One sentence if relevant: "I took time away from the workforce to care for a family member and am now fully focused on returning." |
| Layoff or job loss | List end date of prior role accurately. Do not extend dates to cover the gap. | Not required to address unless the gap is over 12 months. One factual sentence suffices if addressed. |
| Health or medical | Not required to disclose on resume or cover letter. List end date accurately and leave the gap undescribed. Coach the user that they are not obligated to explain medical gaps to employers. | Not required to address. |
| Education or reskilling | List the program, institution, and dates under Education or Certifications. This converts the gap into a credential. | Mention if directly relevant to the target role. |
| Freelance or consulting | List as "Independent Consultant" or "Freelance [Function]" with date range and one to two bullets describing the work. Requires at least one real engagement to support this framing. | Frame as deliberate: the candidate chose independent work and is now seeking a return to a full-time role. |
| Entrepreneurship | List the venture by name with dates and bullets describing what was built, even if it did not succeed. Entrepreneurship is an asset, not a gap. | Frame the experience as directly transferable. |
| Personal sabbatical or travel | List as "Career break: personal sabbatical" with date range if over 12 months. Under 12 months, leave undescribed. | Not required to address unless the gap is significant. |
| Unknown or user declines to share | List end dates accurately. Do not fabricate activity to cover the period. Flag to the user that a gap with no framing may prompt recruiter questions and offer to help them prepare a verbal answer. | Not required. |

**Never do the following:**
- Extend employment dates to cover a gap. This is resume fraud and a terminable offense if discovered.
- Use vague language like "career exploration" without the user explicitly approving that framing.
- Pressure the user to disclose gap reasons they are not comfortable sharing.
- Treat a gap as inherently damaging. Many hiring managers are indifferent to gaps under 12 months and increasingly understand longer gaps too.

**Verbal preparation.** After writing the resume, if a gap is present, flag to the user that it may prompt a recruiter question and offer to prepare a one-sentence verbal answer they can deliver without hesitation. The answer should be factual, forward-looking, and brief.

---

## LinkedIn Profile Optimization

After delivering the resume and cover letter, offer to optimize the user's LinkedIn profile:

> "Would you like me to optimize your LinkedIn profile to match your resume? A well-aligned profile significantly increases recruiter visibility, and I can rewrite your headline, about section, and experience entries to match the positioning we just built."

If the user says yes, proceed using Claude's native capability. There is no separate reference file for this. Use the resume just produced as the source of truth for positioning, language, and tone. Key areas to address: headline (role title plus one differentiator, not just a job title), about section (first-person, three to four sentences, mirrors the resume summary), experience entries (aligned with resume bullets but written in LinkedIn's more conversational register), and skills section (prioritize skills that appear in the target job descriptions the user has shared).

Do not replicate the resume verbatim. LinkedIn is a different surface with a different register. The resume is formal and document-structured. LinkedIn is more direct and human.

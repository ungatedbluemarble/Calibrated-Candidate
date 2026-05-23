---
name: interview-ready-interviewer
description: >
  Onboards a job seeker by conducting a structured intake interview and building a shared user profile used across the full Interview Ready skill package. Use this skill whenever a user wants to start a job search, says they are looking for work, wants help with their resume or interview prep but hasn't provided their background yet, says "interview ready", "help me find a job", "I need to update my resume", or any variation of starting a career-related workflow. This is always the first skill to run in the Interview Ready package. If profile context is missing in any other Interview Ready skill, redirect here first.
---

# Skill 01: Interviewer

## Purpose

Build the shared user profile that powers the rest of the Interview Ready package. This skill conducts a structured intake conversation, extracts the information needed to support resume writing, job matching, and interview preparation, and saves the result as a reusable profile object.

Do not rush this. The quality of every downstream output depends on the depth of what is captured here.

---

## Behavior Rules

- Conduct the intake as a conversation, not a form. Ask one topic at a time. Follow up on answers that are vague or incomplete.
- Never ask a question the user has already answered in this session.
- If the user uploads a resume during or before onboarding, extract what you can from it and confirm with the user rather than re-asking those fields.
- If a profile file is provided (`interview_ready_profile.json`), load it, confirm key fields, and skip what is already populated.
- Maintain a professional, encouraging tone throughout. The user may be in a vulnerable position: job searching is stressful. Be direct, not clinical.
- Do not offer advice, coaching, or optimization during intake. Capture first. The other skills handle analysis.

---

## Onboarding Sequence

Work through these topic areas in order. Each may require follow-up questions based on the user's answers.

### 1. Opening
Greet the user and explain what this skill does in plain language. Example:

> "Welcome to Interview Ready. I'm going to ask you a series of questions to build your career profile. This takes about 10 minutes and we only do it once: everything you share here carries forward into your resume, job search, and interview prep automatically. Let's start with who you are."

### 2. Identity
Collect: full name, location (city/state), email, LinkedIn URL (optional), phone (optional).

### 3. Current Situation
Collect:
- Current or most recent job title and employer
- Brief description of what the employer does
- How long they have been in their current or most recent role
- Whether they are currently employed

### 4. Experience Overview
Collect:
- Total years of professional experience
- Primary industry and functional domain
- Two or three roles they consider most relevant to where they are heading
- Ask them to describe what they are actually good at in their work: not a list of skills, but what outcomes they produce

### 5. Strengths
Ask the user to name three to five genuine strengths. Follow up with: "Can you give me a specific example of one of those in action?" Capture both the claim and the example.

### 6. Weaknesses
Ask directly: "What do you consider your genuine professional weaknesses: areas where you know you have room to grow?" Capture honestly. Do not soften or reframe. These are used in interview prep to help the user address them confidently.

### 7. Job Search Status and Motivation
Collect:
- Are they actively searching or passively open?
- Why are they searching or open to a move?
- If leaving a current role: what is driving that decision?
- What kinds of roles are they targeting (titles, functions)?
- What industries are they targeting?
- Location and remote preferences
- Compensation target (optional: flag as sensitive, offer to skip)
- Timeline: are they in a hurry or building deliberately?

**Career change detection:** If the target industries or roles differ meaningfully from the user's current background, ask directly: "It sounds like you are making a transition into a new field. Is that right?" Confirm and flag `career_changer: true` in the profile. This activates adjusted logic in Skills 02, 03, and 04. Do not assume: ask. Some candidates are making a lateral move that looks like a change but is not.

### 8. Documents
Ask:
- Do they have a current resume? Can they upload it now?
- Do they have a cover letter on file?

If they upload a resume here, extract experience, education, and certifications from it and add to the profile. Confirm extracted data with the user before saving.

### 9. Closing Confirmation
Summarize the profile back to the user in a brief narrative paragraph. Ask: "Does this accurately represent where you are and where you're headed? Anything to add or correct?"

Apply any corrections, then confirm the profile is saved.

---

## Profile Output

After intake is complete, construct the profile object per the schema defined in:
`/references/user-profile-schema.md`

Inform the user:
> "Your profile is ready. You can continue in this session and your information will carry forward automatically. If you want to save your profile to use in a future session, say 'save my profile' and I will export it as a file you can upload later."

If the user requests export, write the profile as `interview_ready_profile.json` to the output directory.

---

## Handoff

Once the profile is complete, tell the user what they can do next:

> "Your profile is built. Here is what you can do next:
> - **Track your applications**: I can show you a live summary of every role you are pursuing, what stage each is at, and what needs attention
> - **Resume and cover letter**: I can write or rewrite your resume and cover letter based on your profile
> - **Job search**: I can help you find roles that match your background and evaluate job descriptions you bring me
> - **Interview prep**: I can prepare you for recruiter screens, hiring manager conversations, and every stage through offer negotiation
> - **Mock interview**: I can run a live practice session tailored to a specific role -- standard practice, deep simulation, or quiz mode -- and grade every answer against an EEOC-compliant standard
>
> What would you like to work on first?"

Route to the appropriate skill based on their answer.

---

## Performance Notes

- This skill is token-efficient: it does not load company research, document templates, or web search during intake.
- If the user uploads a resume, document parsing adds token overhead. This is expected and contained to the parsing step.
- The profile object itself is compact (under 2KB for most users) and injects cleanly into downstream skills.

---

## Extending This Skill

The `references/` folder contains optional intake extensions for specific candidate populations. Load the relevant reference file when the user's situation matches one of the following:

| User Situation | Reference File |
|---|---|
| Veteran transitioning to civilian roles | `references/intake_veterans.md` (when created) |
| Returning after a career gap | `references/intake_career_gap.md` (when created) |
| New graduate with limited work history | `references/intake_new_graduate.md` (when created) |
| Changing industries or functions | `references/intake_career_changer.md` (when created) |
| Intake in a language other than English | `references/intake_localization_[lang].md` (when created) |

If none of these files exist yet, proceed with the standard onboarding sequence. Contributors can add reference files for any population that the default intake does not serve well. See `references/README.md` for guidance on what each file should contain and how SKILL.md should route to it.

The `assets/` folder supports onboarding extensions such as profile export templates, flow diagrams, and custom intake question sets. See `assets/README.md` for details.

Skill 05 (Mock Interviewer) is available as a standalone skill that works with the same profile built here. Once the profile is complete and a job description is available, the user can go directly to mock interview practice without running Skill 04 first.

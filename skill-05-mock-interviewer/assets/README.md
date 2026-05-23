# assets/ -- Skill 05: Mock Interviewer

This folder is reserved for assets that extend the mock interview experience -- question banks, scoring rubrics, simulation templates, and industry-specific content.

---

## What Belongs Here

**Quiz question banks**
Structured sets of multiple choice and open-answer questions for Quiz Mode (Mode 3). Questions should be organized by category: company knowledge, industry knowledge, role-specific knowledge, and candidate self-knowledge. Each question should include the correct answer and a one-sentence explanation. Place as `quiz-bank-general.md` or `quiz-bank-[industry].md`.

**Scoring rubric variants**
The default grading standard is defined in SKILL.md. If you develop calibrated rubrics for specific role types or seniority levels -- for example, a rubric that weights strategic thinking more heavily for director-level candidates -- place them here as `rubric-director.md`, `rubric-technical-ic.md`, and so on.

**Simulation session templates**
Pre-built session structures for common interview formats -- a 30-minute phone screen, a 60-minute hiring manager conversation, a 90-minute panel with three interviewers. Each template defines the question count, stage sequence, follow-up cadence, and feedback timing. Place as `template-phone-screen.md`, `template-panel-90min.md`.

**Compensation and offer quiz content**
Quiz Mode questions specifically for offer negotiation preparation -- market rate knowledge, negotiation vocabulary, total compensation components, and common employer tactics. Place as `quiz-bank-offer-negotiation.md`.

---

## What Does Not Belong Here

- Reference documents that guide Claude's question generation logic -- those go in `references/`
- Personal session data or answer logs from real users
- Files larger than a few hundred KB -- keep assets lightweight

---

## Contributing

If you build a question bank, scoring rubric, or session template that improves the mock interview experience for a specific audience, open a pull request with the file and a note on what it covers, how it was validated, and when the skill should load it.

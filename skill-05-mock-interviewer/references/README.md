# references/ -- Skill 05: Mock Interviewer

This folder is reserved for reference documents that guide how Claude generates questions, grades answers, and simulates interviewers for specific contexts.

---

## What Belongs Here

**EEOC compliance reference**
A structured summary of EEOC-prohibited question topics, job-relevance validation logic, and compliant rewrite examples for common problematic questions. Place as `eeoc-compliance-reference.md`. The skill loads this when generating questions to validate compliance before asking them.

**Industry-specific question banks**
Curated sets of behavioral, situational, and technical questions for specific industries or functional domains. Each file should cover one domain and include 20 to 40 questions with the competency each one assesses documented. Name them: `questions-technology.md`, `questions-finance.md`, `questions-healthcare.md`, and so on.

**Role-level question calibration guides**
Guidance on how question depth, probing style, and answer expectations differ by seniority level. IC questions test execution. Manager questions test delegation and development. Director and above questions test strategy and influence. Place as `question-calibration-by-level.md`.

**Interviewer persona profiles**
Fictional but realistic interviewer profiles for each pipeline stage that give the simulation more texture. A technical panel interviewer profile differs significantly from a Chief Revenue Officer final round profile. Place as `persona-technical-panelist.md`, `persona-cro-final-round.md`, and so on.

**International interview convention guides**
Interview norms vary significantly by country. Question style, answer length expectations, formality, and what signals competence all differ. If you extend this skill for international users, place country-specific guides here: `conventions-uk.md`, `conventions-germany.md`, `conventions-japan.md`.

---

## What Does Not Belong Here

- Assets used in document generation or scoring display -- those go in `assets/`
- The user profile schema -- that is at `references/user-profile-schema.md` (already present)
- Personal data from real users or real interviews

---

## Contributing

If you build a question bank, persona profile, or convention guide that meaningfully improves simulation quality for a specific role type, industry, or candidate population, open a pull request with the file and a note on what it covers and when the skill should load it.

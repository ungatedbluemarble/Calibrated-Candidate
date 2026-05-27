# Calibrated Candidate: Changelog

---

## May 26, 2026

**Skill updated:** Interview Prep (skill-04-interview-prep)
**Closes:** Issue #11

**What changed:** Automatic source health checking added for compensation and salary law sources.

Skill 04 uses eight compensation data sources and five salary law sources to generate offer benchmarking and jurisdiction-specific salary law guidance. Previously there was no mechanism to detect or respond to source degradation when a source became paywalled, restructured, or discontinued.

The skill now manages source health automatically using a `source_health_cache` object in the user profile. On first use the cache is empty and the skill always runs a full source check before generating compensation guidance. On subsequent uses the skill checks whether 90 days have passed since the last check and runs a fresh check if so. No manual scheduling or external reminders are required. The review cadence is tied to actual usage and persists across sessions as long as the user saves and reloads their profile.

When a source check detects a degraded or failed source, the skill notifies the user before generating guidance rather than silently skipping the source.

A new reference document `quarterly-source-review.md` has been added to `skill-04-interview-prep/references/`. It defines verification criteria for all thirteen sources, pass/degraded/failed classifications, replacement procedures, and a results log table.

**What to do:** Re-upload skill-04-interview-prep to replace the previous version. Add `quarterly-source-review.md` to the `skill-04-interview-prep/references/` folder. Your profile and pipeline are not affected. On your next compensation or salary law session the skill will run a source check automatically since no cache exists yet.

---

## May 26, 2026

**Skill updated:** Interview Prep (skill-04-interview-prep)
**Closes:** Issue #11

**What changed:** Quarterly source review protocol added for compensation and salary law sources.

Skill 04 uses eight compensation data sources and five salary law sources to generate offer benchmarking and jurisdiction-specific salary law guidance. These are live external sources that can become paywalled, restructured, or discontinued without any visible failure signal to the user. Previously there was no mechanism to detect or respond to source degradation.

A quarterly review protocol has been added covering all thirteen sources. The protocol defines the review cadence (aligned to US fiscal quarters), verification criteria for each source, what constitutes a passing, degraded, or failed source, and what to do when a source changes status. The full protocol lives at `skill-04-interview-prep/references/quarterly-source-review.md`.

The SKILL.md has been updated in two places. The salary law search section now includes a source health note instructing the skill to flag degraded sources to the user rather than silently skipping them. The offer negotiation compensation research section now includes the same instruction, with a reference to the protocol document for current source status.

**What to do:** Re-upload skill-04-interview-prep to replace the previous version. Add `quarterly-source-review.md` to the `skill-04-interview-prep/references/` folder in your local installation. Create a recurring quarterly GitHub issue labeled `quarterly-review` and `skill-04` to trigger the review at the start of each quarter. Your profile and pipeline are not affected.

---

## May 26, 2026

**Skill updated:** Resume Writer (skill-02-resume-writer)

**What changed:** Five improvements based on real-world usage feedback.

**Page break control.** The skill now places an explicit page break before the Professional Experience section when the resume runs longer than one page. Previously the skill relied on Word's automatic pagination, which could orphan a section header or job title at the bottom of a page with its content on the next. The break is now intentional and always results in page one ending with complete content and page two starting cleanly at a section header.

**Context-aware section removal.** Core Competencies is now optional rather than always included. The skill drops it automatically when the target company is a startup or early-stage organization, or when the JD emphasizes execution, speed, or low process overhead. When the section is removed, technical tools and platforms are compressed into a single line at the bottom of the resume rather than being lost entirely.

**Word economy rule.** A hard rule was added requiring every word to earn its place. The test is: does removing this word change the meaning? If not, remove it. Bullets over two lines must be restructured or split. This produces tighter, more confident output.

**Cover letter tone rules.** The skill now prohibits insider jargon, regulatory acronyms, and technical abbreviations that a hiring manager outside the candidate's industry would not recognize. A tone check step was added on the opening sentence to catch openers that sound like claims about the candidate rather than genuine engagement with the company.

**Cover letter structure.** The paragraph structure was updated to require a dedicated paragraph about why the candidate wants to work at this specific company. This paragraph must not be transferable to a different company without changes. The previous structure focused entirely on the candidate's accomplishments without requiring any genuine company-specific interest to be expressed.

**What to do:** Re-upload skill-02-resume-writer to replace the previous version. Your profile and pipeline are not affected.

---

## May 26, 2026

**Skill updated:** Job Search Expert (skill-03-job-search)

**What changed:** Role validation added to proactive job search.

When the Job Search Expert searches for and recommends roles on your behalf, it now verifies each role before presenting it to you. Previously the skill could surface roles that were closed, location-restricted, or had broken application links without detecting the problem.

Three checks now run on every role before it reaches you. The skill confirms the posting is actively accepting applications, confirms remote eligibility matches your constraints, and confirms the application path resolves to a working form. Roles that fail any check are removed and replaced. If fewer roles can be verified than requested, the skill tells you how many it confirmed rather than padding the list with unverified results.

A No Results Protocol was also added. When a search returns fewer confirmed open roles than requested, the skill now diagnoses which constraint is most likely narrowing the field, asks one targeted question framed as a choice, and waits for the user's answer before running a new search. If two rounds of adjustment still produce no results, the skill offers a strategy shift rather than ending the session. The skill will never tell a user to search on their own.

The existing JD evaluation path (when you bring a role to the skill via paste or URL) already handled closed role detection. This update extends the same standard to roles the skill finds on its own.

**What to do:** Re-upload skill-03-job-search to replace the previous version. Your profile and pipeline are not affected.

---

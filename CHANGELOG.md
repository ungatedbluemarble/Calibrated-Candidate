# Calibrated Candidate: Changelog

---

## May 29, 2026

**Schema updated:** Shared User Profile (all skills)
**Skills updated:** Interviewer (skill-01-interviewer), Resume Writer (skill-02-resume-writer), Job Search (skill-03-job-search)

**What changed:** Experience bullets promoted from plain text to structured evidence records, and the no-invented-numbers rule moved into the profile data itself.

Previously each entry in `experience[].bullets` was a bare string. A string cannot be referenced, scored, or traced, so every skill that needed to reason about a specific accomplishment had to re-read the prose and re-derive its judgment each time. There was no shared, durable record of which bullets a candidate could actually defend in an interview, and the no-invented-numbers rule lived only as writing guidance in Skill 02 rather than as a fact carried in the profile.

Each bullet is now a structured object with a stable `id` (in the form `E{role}-B{bullet}`), the bullet `text`, a `skills` array of the competencies it evidences, a `metric` field for any quantified result, a `metric_verified` boolean, and an `evidence_strength` rating of `defensible`, `soft`, or `unverified`. This makes every accomplishment individually addressable across the pipeline: a bullet can be discussed, reordered, or mapped to a job requirement by ID rather than by re-quoting it, and the candidate's ability to defend each claim is recorded once and reused everywhere.

The schema is now version 1.1. The only change from 1.0 is the bullet shape. A lossless migration rule runs at profile load: if a bullet is still a string, it is wrapped into the new object with the original text preserved verbatim and the remaining fields defaulted, then the version is bumped. No saved 1.0 profile breaks, and skills that read only the bullet text continue to work unchanged against both versions.

Skill 01 now captures the structured bullet fields at intake, so a candidate who interviews before uploading a resume still produces addressable evidence. When the user states a number in conversation it is recorded as verified, because the user just confirmed it; numbers are never inferred or estimated.

Skill 02 now records `evidence_strength` and `metric_verified` on every bullet during parsing and tailoring. This is the same no-invented-numbers rule as before, now written into the profile so the rest of the pipeline inherits the judgment instead of re-deriving it. A bullet rated `unverified` is never promoted into a tailored resume or led with in positioning until the user confirms it, and any strong bullet held back for this reason is surfaced in the existing "Numbers needed" section.

Skill 03 now maps job description requirements against the per-bullet `skills` tags and scores fit by evidence strength. A requirement is marked Met only when a `defensible` bullet covers it; a requirement backed only by a `soft` or `unverified` bullet is Partial at best, and the gaps section notes what the candidate would need to confirm to make it defensible. Unverified bullets are never used to clear a hard requirement.

Skills 04, 05, and 06 consume the profile but did not need instruction changes. They receive the updated schema file so all skills share one version of the contract.

**What to do:** Replace `user-profile-schema.md` in all seven locations: `shared/` and each `skill-NN/references/` folder. Re-upload skill-01-interviewer, skill-02-resume-writer, and skill-03-job-search to replace the previous versions. Skills 04, 05, and 06 need only the replaced schema file in their reference folders. Existing saved profiles are migrated automatically on next load; you do not need to edit any profile by hand.

---

## May 29, 2026

**Skill updated:** Job Search (skill-03-job-search)

**What changed:** Proactive Role Validation hardened against stale and closed postings.

The previous validation logic treated a rendered job description with a visible apply button as sufficient proof that a role was open. Applicant tracking systems and aggregators routinely keep serving the full description and a non-functional apply control after a requisition closes, so this produced a false positive: a closed JD Power role was presented as open because the fetched ATS page returned an active-looking description, and a follow-up aggregator link carrying a plain "this job has closed" flag was passed to the user without the flag being caught.

Check 1 now treats the body of a posting as untrusted for open/closed status and requires three steps before a role is scored open: a full-page closed-signal scan (any closed signal anywhere on the page closes the role, regardless of the JD body), an age heuristic (postings older than roughly 45 days in fast-moving categories are presumed closed absent an explicit open indicator), and a source-trust ranking (company ATS over aggregators). A new conflict rule states that closed always wins when any signal conflicts.

A new Check 4 requires a per-role confirmation line disclosing what was actually verified and how (confirmed open via company ATS versus not independently confirmed via aggregator only), plus remote status, posting age, and check date, so validation is never presented as more certain than it is.

Bulk-mode JD evaluation now points explicitly at the full four-check process rather than referring to it loosely.

**What to do:** Re-upload skill-03-job-search to replace the previous version. No profile or pipeline impact.

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

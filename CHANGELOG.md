# Calibrated Candidate: Changelog

---

## May 26, 2026

**Skill updated:** Job Search Expert (skill-03-job-search)

**What changed:** Role validation added to proactive job search.

When the Job Search Expert searches for and recommends roles on your behalf, it now verifies each role before presenting it to you. Previously the skill could surface roles that were closed, location-restricted, or had broken application links without detecting the problem.

Three checks now run on every role before it reaches you. The skill confirms the posting is actively accepting applications, confirms remote eligibility matches your constraints, and confirms the application path resolves to a working form. Roles that fail any check are removed and replaced. If fewer roles can be verified than requested, the skill tells you how many it confirmed rather than padding the list with unverified results.

The existing JD evaluation path (when you bring a role to the skill via paste or URL) already handled closed role detection. This update extends the same standard to roles the skill finds on its own.

**What to do:** Re-upload skill-03-job-search to replace the previous version. Your profile and pipeline are not affected.

---

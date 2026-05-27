# Quarterly Source Review Protocol
## Skill 04: Interview Prep

This document governs the quarterly review of all compensation and salary law sources used by Skill 04. It defines the review cadence, the verification criteria for each source, what constitutes a passing versus failing source, and what to do when a source degrades or fails.

---

## Why This Exists

Skill 04 uses eight compensation data sources and five salary law sources to generate offer benchmarking and jurisdiction-specific salary law guidance. These are live external sources. Data models, access policies, and site structures change over time. A source that becomes paywalled, restructured, or discontinued produces inaccurate or incomplete guidance without any visible failure signal to the user. Quarterly review prevents silent degradation.

---

## How the Skill Triggers This Review

The skill manages source health automatically using a `source_health_cache` object in the user profile. No manual scheduling is required.

On first use the cache is empty. The skill always runs a full source check before generating any compensation or salary law guidance and writes the results to the cache with the current date as `last_checked`.

On subsequent uses the skill reads `last_checked` from the cache and calculates whether 90 days have passed. If yes, it runs a full source check and updates the cache. If no, it uses the cached results and proceeds.

This means the review happens automatically, is tied to actual usage rather than a calendar, and persists across sessions as long as the user saves and reloads their profile. A user who has not used the compensation features in over a quarter will automatically trigger a fresh check on their next use.

The maintainer does not need to create recurring GitHub issues or external reminders. The skill handles cadence internally.

---

## Review Cadence

Reviews run automatically every 90 days, triggered by the skill's `source_health_cache` logic. The approximate quarterly windows are:

| Quarter | Approximate Window |
|---|---|
| Q1 | January |
| Q2 | April |
| Q3 | July |
| Q4 | October |

The exact date shifts based on when the user last triggered a compensation or salary law session. The skill handles the cadence internally.

---

## Compensation Sources: Verification Criteria

For each source, verify the following before marking it as passing.

| Source | URL | What to Verify |
|---|---|---|
| Levels.fyi | levels.fyi | Role search returns salary data without requiring login. TC data is populated for common tech roles. No paywall on base results. |
| Glassdoor | glassdoor.com/Salaries | Salary search returns a range without requiring a paid account. Data is populated for the role types Skill 04 targets. |
| LinkedIn Salary | linkedin.com/salary | Returns salary ranges for role and location combinations without requiring Premium. If Premium is now required, flag as degraded. |
| Payscale | payscale.com | Returns a salary report for a role without requiring full profile completion or payment. If gated behind a survey wall, flag as degraded. |
| Bureau of Labor Statistics | bls.gov/oes | OES data pages load and return current occupational wage tables. Data vintage is within the last 12 months. |
| Dice | dice.com/salary-calculator | Calculator returns a salary estimate for a role without requiring account creation. |
| Built In | builtin.com/salaries | Salary data pages load and return ranges without a login wall. |
| Indeed Salary | indeed.com/career/salaries | Salary search returns data without requiring account creation. |

**Passing criteria:** Source returns usable salary data for a common role query without requiring payment, account creation, or full profile completion.

**Degraded criteria:** Source requires login, survey completion, or payment to return data that was previously free. Flag as degraded but keep in the list with a note.

**Failed criteria:** Source returns no data, returns an error, has shut down, or has restructured such that it no longer serves the use case. Remove from the active list and replace.

---

## Salary Law Sources: Verification Criteria

| Source | URL | What to Verify |
|---|---|---|
| State labor department websites | Varies by state | At least five major state labor department sites load and contain current salary history or expectation ban law information. Spot-check: CA, NY, IL, TX, WA. |
| National Conference of State Legislatures | ncsl.org | Salary history ban legislation tracker page loads and shows a current update date within the last 12 months. |
| U.S. Department of Labor | dol.gov | Wage and hour guidance pages load. Pay equity resource pages are accessible without login. |
| EEOC | eeoc.gov | Pay equity and employment discrimination guidance pages load and are current. |
| Ballotpedia | ballotpedia.org | State-level salary law and ballot measure pages load and return current legislative status. |

**Passing criteria:** Source loads, content is current, and the information is accessible without login or payment.

**Degraded criteria:** Source loads but content has not been updated in over 12 months, or access now requires registration.

**Failed criteria:** Source does not load, has been discontinued, or no longer covers salary law topics.

---

## What to Do When a Source Fails or Degrades

**Degraded source:** Add a note in the source table in the README and in this document. Add a comment in SKILL.md next to the source reference flagging the degradation and the date it was identified. Do not remove the source from the skill logic until a replacement is confirmed. Update the user-facing note in Skill 04 to reflect the limitation.

**Failed source:** Remove from the active source list in SKILL.md, the README tables, and this document. Open a replacement search immediately. Criteria for a replacement source: publicly accessible without login or payment, covers compensation or salary law data for the US market, is updated at least annually, and is a recognized or authoritative source in its domain. Document the replacement in the CHANGELOG with the date, the source removed, and the source added.

---

## Skill 04 Logic Update After Review

After each quarterly review, update the following locations if any source has changed status:

1. The source list in `SKILL.md` under the Salary Law Search section and the Compensation Research section of Offer Negotiation
2. The source tables in the main `README.md` under Compensation Research and Salary Law Research
3. This document, updating the verification results table below
4. The `CHANGELOG.md` if any source was added, removed, or flagged

---

## Verification Results Log

Append to this table after each quarterly review. Do not overwrite previous results.

| Review Date | Reviewer | Sources Passing | Sources Degraded | Sources Failed | Notes |
|---|---|---|---|---|---|
| Pending first review | | | | | Initial protocol established May 2026 |

---

## Closing Note

The README salary law section already directs users to their state Department of Labor to verify current status before an interview. The same discipline applies here. This protocol is not a guarantee that sources are current between reviews. It is a structured commitment to checking regularly and being transparent when something changes.

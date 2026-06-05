# Memory Integration Contract
## Calibrated Candidate: Shared Persistence and Drift Model

This document defines how the Calibrated Candidate skills persist user state, how they
use Claude's memory filesystem when it is available, and the drift and anti-hallucination
rules that govern every write. It is the single source of truth for this behavior. Every
skill references this file rather than restating the rules. No skill may define persistence
or drift behavior that contradicts this contract.

If you are reading this to understand one skill, read this file first, then the
persistence and drift sections in that skill's SKILL.md, which point back here.

---

## 1. The persistence model: JSON is canonical, memory is an optional cache

Calibrated Candidate runs on more than one Claude surface. It runs on claude.ai, on the
Claude desktop and mobile apps, in Claude Code, and anywhere else these skills are
installed. The memory filesystem does not exist on all of those surfaces, and where it
does exist it is off by default until the user turns it on. The portable profile file is
the only persistence mechanism that works everywhere.

Therefore:

- The portable profile file, `calibrated_candidate_profile.json`, is the canonical source
  of truth. It is defined by `shared/user-profile-schema.md`. Every skill can always read
  and write it. The product works fully with the JSON alone and no memory.
- Claude's memory filesystem, when available and enabled, is a write-through convenience
  cache layered on top of the JSON. It lets a returning user be recognized without
  re-uploading their profile, and it keeps the live pipeline current between sessions on
  that surface. It never replaces the JSON.
- On any conflict between memory and the JSON, the JSON wins. Memory is reconciled to
  match the JSON, never the other way around. A skill that detects disagreement surfaces
  it to the user per the reconciliation rule in section 4 rather than silently trusting
  memory.

This is a hard architectural rule. Do not write a skill that treats memory as primary or
that assumes memory is present. A user with memory off, or on a surface without it, must
get the complete product through the JSON.

---

## 2. Detecting memory and prompting the user to enable it

Memory is opt-in. A user on a memory-capable surface may still have it switched off, in
which case the skill cannot see or write memory files until they enable it. Skills handle
this in three steps, without breaking the workflow.

**Step 1: Detect.** At the start of a session, attempt to list the memory namespace
(section 3). One of three states results:

- Memory available and populated: a returning user. Load from memory, then reconcile
  against any JSON the user provides (section 4).
- Memory available but empty: either a new user, or a returning user whose data has not
  been cached on this surface yet. Proceed and write through to memory as state is built.
- Memory unavailable: the surface does not support it, or the user has it switched off.
  Fall back to JSON-only operation.

**Step 2: Prompt once when memory is unavailable.** When memory is unavailable, tell the
user plainly that turning it on gives them persistence across sessions, and that without
it they will need to save and re-upload their profile file each time. Use language like:

> "Calibrated Candidate can remember your profile and your live application pipeline
> between sessions if you turn on memory. It is off by default. You can enable it in your
> Claude settings under Capabilities, then come back and I will pick up automatically.
> Without memory, everything still works: I will give you a profile file to save and
> re-upload at the start of each session instead."

Prompt once per session at most. If the user does not want to enable it, proceed with the
JSON path and do not ask again in that session.

**Step 3: Degrade gracefully.** Memory is best-effort. If a memory read or write fails
mid-session, do not break and do not show the user an error trace. Continue on the JSON
path and, if useful, note once that memory did not update and their saved profile file
remains the reliable record.

---

## 3. Memory namespace layout

When memory is available, Calibrated Candidate uses a dedicated set of paths so its state
is self-contained and the close-out (section 6) is surgical. Each fact lives in exactly
one file (section 4, single source of truth).

- `/profile.md`
  Light identity anchor only: name, location, current role and employer, and durable
  facts a returning user would expect to be remembered. This is not a place for search
  data. It mirrors the `identity` and the stable parts of `background` from the JSON.

- `/areas/job-search.md`
  Strategy, reusable across multiple searches: target lanes, hard constraints, exclusions,
  fit pattern. Carries an `ACTIVE` or `DORMANT` flag. Mirrors `search_status` from the JSON.

- `/areas/job-search-pipeline.md`
  The live application tracker, one entry per application: company, role, stage, status,
  next step, action items. This is the high-churn file. It mirrors `interview_history`
  from the JSON. Sensitive identifiers and confidential figures are excluded per section 4.

- `/areas/archive/job-search-<YYYY-MM-DD>.md`
  Close-out archive (section 6). A dated snapshot of the pipeline and strategy at the time
  the user was hired or stopped a search. Active skills never read this path by default.

The JSON remains the complete record. Memory holds only what these paths describe, mapped
back to the canonical JSON fields above.

---

## 4. Drift and anti-hallucination contract

This contract governs every write to memory and every write to the JSON, in all six
skills. No skill may violate it.

### Data drift (keeping the record internally consistent)

- **Tag discipline.** Record `[stated]` only for what the user actually said. An inference
  you drew, or an option you proposed, is not a stated fact. If the user picks an option
  you offered, their choice is stated; your reasoning behind it is not.
- **Single source of truth.** Each fact lives in exactly one place. An application's status
  lives in the pipeline record and nowhere else, so it cannot disagree with itself. The
  memory pipeline file mirrors the JSON `interview_history`; it is not a second independent
  copy that can drift.
- **Read before write.** Every update reads the current state and reconciles. Never blind-
  append. Preserve history rather than silently overwriting: record "now X, previously Y"
  when a fact changes.
- **Reconciliation.** When two sources disagree (memory vs JSON, or two records), surface
  the conflict to the user and let them resolve it. Do not silently pick one. The one
  automatic rule is the architectural tie-breaker in section 1: if the user does not
  resolve it, the JSON value stands.

### Semantic drift (not inventing things)

- **Provenance.** Every claim a skill writes or renders traces to a user-stated source.
  Numbers and credentials carry where they came from.
- **Ask, do not invent.** Fill a gap by asking the user, never by fabricating a value.
- **Corrected once, gone forever.** When the user corrects a value, the old form is retired
  permanently and must never resurface anywhere, in any later session or document. Remove
  it; do not soften it to "previously."
- **Confidentiality.** Never write employer-confidential outcomes into the record; omit
  rather than approximate. Self-reported metrics keep their "self-reported" qualifier.
  Compensation figures and sensitive personal identifiers are excluded from the memory
  pipeline file; they remain in the user-controlled JSON only.

### Honest limit

Memory is best-effort. These rules make drift far less likely and far more catchable; they
do not make it impossible. The pipeline-dashboard audit (the skill that owns the memory
audit) exists because no write discipline is perfect. When memory is unavailable, fall back
to the JSON and to asking the user rather than breaking.

---

## 5. Fictitious-data rule for skill content

All businesses and people that appear anywhere in skill text, reference files, examples,
templates, and demos are fictitious. The only real specifics that ever appear are the ones
the end user provides about themselves during their own session, which live in their
profile and their memory, never in shipped skill files. When an example needs a company, a
recruiter, a comp figure, or a candidate, invent one. Never cite a real company or person.
This matches the existing Jane Doe / fictitious-company convention already used in the
repository's reference examples.

---

## 6. Close-out lifecycle (archive by default, user-confirmed)

Owned and executed by the pipeline-dashboard skill. Stated here so every skill shares the
definition.

- **Trigger:** the user states they accepted or started a job. A mention is not a trigger
  by itself: "I got a job" is not always "stop searching." Confirm intent before acting.
  Some users keep searching or want the record kept live.
- **Never silent, never destructive without confirmation.** The skill prompts; it does not
  auto-clear. It offers both options plainly: archive the search history so it is out of
  the way but recoverable, or delete it entirely.
- **Archive (default):** write the new role to `/profile.md`, move the pipeline and a
  strategy snapshot to `/areas/archive/job-search-<date>.md`, and stop surfacing them in
  the active dashboard. Update the JSON to match.
- **Delete (explicit choice only):** write the new role to `/profile.md`, remove the
  pipeline, and reset the strategy file. Update the JSON to match.
- **Isolation guarantee (hard rule, part of this contract):** archived search data is never
  read by any active skill or surfaced in any conversation unless the user explicitly
  reactivates that search. Archive-by-default is acceptable only because this rule holds.
  The dated archive path under `/areas/archive/` exists to make this isolation structural
  rather than dependent on instruction discipline alone.

---

## 7. Relationship to the JSON schema

This contract governs persistence behavior and drift. The shape of the data itself is
defined in `shared/user-profile-schema.md`, which remains the canonical schema. The memory
paths in section 3 mirror specific JSON fields; when the schema changes, this mapping is
updated alongside it. The two files are kept in sync: the schema defines what the data is,
this contract defines how it persists and how drift is prevented.

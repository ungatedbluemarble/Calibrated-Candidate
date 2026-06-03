# Shared User Profile Schema
## Calibrated Candidate: Cross-Skill Context Object

This schema defines the canonical user profile object built by Skill 01 and consumed by Skills 02, 03, and 04. Every skill that needs user context pulls from this structure: nothing is re-asked if it already exists here.

---

## Profile Object

```json
{
  "profile_version": "1.2",
  "created_at": "ISO8601 timestamp",
  "updated_at": "ISO8601 timestamp",

  "identity": {
    "full_name": "",
    "location": "",
    "email": "",
    "linkedin_url": "",
    "phone": ""
  },

  "background": {
    "current_title": "",
    "current_employer": "",
    "current_employer_description": "",
    "years_of_experience": null,
    "industry": "",
    "functional_domain": "",
    "summary_statement": ""
  },

  "experience": [
    {
      "title": "",
      "employer": "",
      "start_date": "",
      "end_date": "",
      "bullets": [
        {
          "id": "E1-B1",
          "text": "",
          "skills": [],
          "metric": null,
          "metric_verified": false,
          "evidence_strength": ""
        }
      ]
    }
  ],

  "education": [
    {
      "institution": "",
      "degree": "",
      "field": "",
      "year": null
    }
  ],

  "certifications": [],

  "strengths": [],

  "weaknesses": [],

  "search_status": {
    "actively_searching": null,
    "career_changer": null,
    "reason_for_searching": "",
    "reason_for_leaving": "",
    "target_industries": [],
    "target_roles": [],
    "target_locations": [],
    "remote_preference": "",
    "compensation_target": "",
    "timeline": ""
  },

  "salary_law_cache": {
    "state": "",
    "city": "",
    "history_ban": null,
    "expectation_ban": null,
    "summary": "",
    "source_url": "",
    "last_searched": "",
    "next_refresh_due": ""
  },

  "documents": {
    "resume_uploaded": false,
    "resume_path": "",
    "resume_format": "",
    "cover_letter_on_file": false
  },

  "extracted_documents": {
    "source_resume": {
      "markdown": "",
      "extraction_method": "",
      "extracted_at": ""
    },
    "source_cover_letter": {
      "markdown": "",
      "extraction_method": "",
      "extracted_at": ""
    },
    "source_jd_skill02": {
      "markdown": "",
      "extraction_method": "",
      "extracted_at": ""
    }
  },

  "connectors": {
    "email": {
      "connected": false,
      "provider": ""
    },
    "storage": {
      "connected": false,
      "providers": []
    },
    "calendar": {
      "connected": false,
      "provider": ""
    },
    "other": ""
  },

  "interview_history": [
    {
      "company": "",
      "role": "",
      "stage": "",
      "interview_date": "",
      "interviewer_name": "",
      "recruiter_email": "",
      "prep_document_generated": false,
      "outcome": "",
      "notes": "",
      "company_research": {
        "research_date": "",
        "funding_status": "",
        "headcount": "",
        "value_proposition": "",
        "recent_news_summary": "",
        "competitive_landscape": "",
        "culture_signals": "",
        "financial_signals": "",
        "interviewers": [
          {
            "name": "",
            "title": "",
            "background_summary": "",
            "value_signals": "",
            "research_date": ""
          }
        ]
      }
    }
  ]
}
```

---

## Experience Bullet Object

Each entry in `experience[].bullets` is a structured evidence record, not a bare string. This lets every skill reference a specific bullet by ID, map it to job requirements, and judge whether the candidate can defend it. The fields:

| Field | Type | Meaning |
|---|---|---|
| `id` | string | Stable handle in the form `E{role}-B{bullet}` (for example `E1-B1` is role 1, bullet 1). Used by Skills 02, 03, and 05 to discuss, reorder, swap, or trace a specific bullet without re-quoting it. Never reused or renumbered once assigned. |
| `text` | string | The bullet as written. This is the only field a 1.0 profile carries, and the only field required for a skill to function. |
| `skills` | array of strings | Discrete skills or competencies this bullet evidences (for example `["Twilio", "SMS compliance", "global telecom"]`). Lets Skill 03 map a JD requirement to evidence directly instead of re-reading prose. |
| `metric` | string or null | The quantified result in this bullet, if any (for example `"reduced onboarding time 40%"`). Null when the bullet carries no number. |
| `metric_verified` | boolean | True only when the user has confirmed the metric is real. A number that was inferred, estimated, or rounded stays false until confirmed. Operationalizes the no-invented-numbers rule at the data layer. |
| `evidence_strength` | string | One of `defensible`, `soft`, or `unverified`. `defensible`: a confirmed, specific claim the candidate can defend in an interview. `soft`: a real but qualitative claim with no hard proof. `unverified`: an inferred or unconfirmed number, or a claim the candidate has not validated. |

A bullet whose `evidence_strength` is `unverified` must not be promoted to a tailored resume or led with in positioning until the user confirms it.

---

## Extracted Documents Object

`extracted_documents` holds clean Markdown extracted from uploaded resumes, cover letters, and job descriptions. Skill 02 writes it after normalizing any uploaded file through the extraction pipeline. Downstream skills read the cached Markdown instead of re-processing the original file.

Each document is a separate object with three fields:

| Field | Type | Meaning |
|---|---|---|
| `markdown` | string | The normalized Markdown extracted from the document. Empty until a successful extraction. |
| `extraction_method` | string | Which path produced the text: `markitdown`, `docx_native`, `ocr_rasterized`, `ocr_direct`, or `user_paste`. Supports failure diagnosis: a cluster of `user_paste` is a signal the automated path is degrading. |
| `extracted_at` | string | ISO 8601 timestamp of the extraction. Per document, not per session. Each document ages on its own clock, so refreshing one does not make the others look stale. |

**Relationship to the `documents` block, and the anti-drift rule:**

The older `documents` block tracks whether a resume exists and where. `extracted_documents` holds what was actually read from it. These two blocks describe the same underlying files and can fall out of sync if updated independently. To prevent drift, any skill that writes `extracted_documents.source_resume` with a successful extraction MUST set `documents.resume_uploaded` to `true` in the same write. Likewise, a successful write to `source_cover_letter` MUST set `documents.cover_letter_on_file` to `true`. The two blocks are always updated together, never separately.

**Scope of `source_jd_skill02`:**

This field is intentionally scoped to Skill 02 tailoring use only. It holds the single job description the resume is being tailored against. Skills 03 and 04 manage their own job description storage in a separate `job_descriptions` array introduced in a later phase. The `_skill02` suffix is deliberate and prevents ownership conflicts between the two. It is not a temporary name.

---

## Version and Migration

Current schema version: `1.2`.

The schema has evolved in two non-destructive steps. `1.0` to `1.1` changed the shape of `experience[].bullets` from a plain string to a structured object. `1.1` to `1.2` added the `extracted_documents` block. Every other field has remained unchanged across all three versions.

**1.1 to 1.2:** A profile that lacks the `extracted_documents` block is pre-1.2. On first write, any skill initializes `extracted_documents` with the three empty document objects shown above. No existing field changes, and a profile with no extracted documents behaves exactly as before. Skills that do not consume extracted documents are unaffected.

**1.0 to 1.1:** In `1.0`, each entry in `experience[].bullets` was a plain string. In `1.1`, each is an object.

**Migration rule, applied at profile load by any skill:**

> If `experience[].bullets[0]` is a string, the profile is version 1.0. Wrap each string `s` at index `i` (zero-based) within role `r` (one-based) as:
> `{ "id": "E{r}-B{i+1}", "text": s, "skills": [], "metric": null, "metric_verified": false, "evidence_strength": "" }`
> Then set `profile_version` to `1.1`. This migration is lossless: the original bullet text is preserved verbatim in `text`.

Skills that read only `text` work unchanged against both versions. Skills that use the new fields populate them as evidence is captured or confirmed; an empty `evidence_strength` means "not yet assessed," which is safe to treat as `soft` for display but never as `defensible`.

---

## Persistence Modes

### Session-Persistent (default)
The profile object lives in the active conversation context. Skills inject the relevant slice when needed. No file is written unless the user requests it.

**Performance note:** Passing the full profile object into every skill call adds token overhead. Skills should request only the fields they need, not the entire object. See the "Minimal Context Slices Per Skill" section below for the required fields per skill.

**Limitation:** Context does not survive across separate conversations. If the user starts a new chat, the profile must be rebuilt or loaded from file.

### File-Based (portable)
The user can export their profile at any point by requesting it. It saves as `calibrated_candidate_profile.json` to their output directory or cloud-connected storage.

On subsequent sessions, the user uploads or references this file and any skill can reload the profile without re-interviewing.

**File load instruction for any skill:**
> "If the user provides a profile file or references a previously saved profile, load it before proceeding. Apply the version migration rule above if the file is version 1.0. Confirm the key fields with the user before continuing: do not assume the file is current."

---

## Field Population Source Map

| Field Group | Populated By |
|---|---|
| identity, background, search_status (including career_changer flag), strengths, weaknesses | Skill 01: Interviewer |
| experience (including bullet `id`, `text`, `skills`, `metric`) | Skill 01 (interview) or Skill 02 (resume parse) |
| experience bullet `evidence_strength` and `metric_verified` | Skill 02: Resume Writer (assigned during parse and on each tailoring pass, per the no-invented-numbers rule) |
| education, certifications | Skill 01 (interview) or Skill 02 (resume parse) |
| salary_law_cache | Skill 04: Interview Prep (auto-updated on first use and each US fiscal quarter) |
| documents | Skill 02: Resume Writer |
| extracted_documents | Skill 02: Resume Writer (written after the extraction pipeline normalizes an uploaded file; updated atomically with the `documents` block per the anti-drift rule) |
| interview_history (including recruiter_email) | Skill 04: Interview Prep (also collected by Skill 01 during connector onboarding for active processes) |
| connectors | Skill 01: Interviewer (connector onboarding step) |

---

## Minimal Context Slices Per Skill

**Skill 02: Resume Writer**
Needs: `identity`, `background`, `experience` (full bullet objects), `education`, `certifications`, `documents`, `extracted_documents`

**Skill 03: Job Search Expert**
Needs: `background`, `strengths`, `weaknesses`, `search_status`, `experience` (titles, domains, and bullet `skills` plus `evidence_strength`)

**Skill 04: Interview Prep**
Needs: `identity`, `background`, `experience`, `strengths`, `weaknesses`, `search_status.target_roles`, `interview_history`, `salary_law_cache`, `connectors.email`

**Skill 05: Mock Interviewer**
Needs: `identity`, `background`, `experience`, `strengths`, `weaknesses`, `search_status.target_roles`, `interview_history`

**Skill 06: Pipeline Dashboard**
Needs: `interview_history`, `connectors`

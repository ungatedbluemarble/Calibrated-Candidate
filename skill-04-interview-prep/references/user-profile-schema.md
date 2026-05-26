# Shared User Profile Schema
## Calibrated Candidate: Cross-Skill Context Object

This schema defines the canonical user profile object built by Skill 01 and consumed by Skills 02, 03, and 04. Every skill that needs user context pulls from this structure: nothing is re-asked if it already exists here.

---

## Profile Object

```json
{
  "profile_version": "1.0",
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
      "bullets": []
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

## Persistence Modes

### Session-Persistent (default)
The profile object lives in the active conversation context. Skills inject the relevant slice when needed. No file is written unless the user requests it.

**Performance note:** Passing the full profile object into every skill call adds token overhead. Skills should request only the fields they need, not the entire object. See the "Minimal Context Slices Per Skill" section below for the required fields per skill.

**Limitation:** Context does not survive across separate conversations. If the user starts a new chat, the profile must be rebuilt or loaded from file.

### File-Based (portable)
The user can export their profile at any point by requesting it. It saves as `calibrated_candidate_profile.json` to their output directory or cloud-connected storage.

On subsequent sessions, the user uploads or references this file and any skill can reload the profile without re-interviewing.

**File load instruction for any skill:**
> "If the user provides a profile file or references a previously saved profile, load it before proceeding. Confirm the key fields with the user before continuing: do not assume the file is current."

---

## Field Population Source Map

| Field Group | Populated By |
|---|---|
| identity, background, search_status (including career_changer flag), strengths, weaknesses | Skill 01: Interviewer |
| experience, education, certifications | Skill 01 (interview) or Skill 02 (resume parse) |
| salary_law_cache | Skill 04: Interview Prep (auto-updated on first use and each US fiscal quarter) |
| documents | Skill 02: Resume Writer |
| interview_history (including recruiter_email) | Skill 04: Interview Prep (also collected by Skill 01 during connector onboarding for active processes) |
| connectors | Skill 01: Interviewer (connector onboarding step) |

---

## Minimal Context Slices Per Skill

**Skill 02: Resume Writer**
Needs: `identity`, `background`, `experience`, `education`, `certifications`, `documents`

**Skill 03: Job Search Expert**
Needs: `background`, `strengths`, `weaknesses`, `search_status`, `experience` (titles and domains only)

**Skill 04: Interview Prep**
Needs: `identity`, `background`, `experience`, `strengths`, `weaknesses`, `search_status.target_roles`, `interview_history`, `salary_law_cache`, `connectors.email`

**Skill 05: Mock Interviewer**
Needs: `identity`, `background`, `experience`, `strengths`, `weaknesses`, `search_status.target_roles`, `interview_history`

**Skill 06: Pipeline Dashboard**
Needs: `interview_history`, `connectors`

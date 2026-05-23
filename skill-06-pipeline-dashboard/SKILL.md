---
name: interview-ready-pipeline-dashboard
description: >
  Tracks and displays the user's active job search pipeline. Use this skill whenever the user asks about their application status, says "where am I with my applications", "show me my pipeline", "what interviews do I have coming up", "which companies am I tracking", "give me a summary of my job search", "add this company to my tracker", or any variation of requesting an overview or update of their active applications. Does not require an active prep session. Requires user profile context from interview-ready-interviewer. If profile is missing, prompt for it before proceeding.
---

# Skill 06: Pipeline Dashboard

## Purpose

Give the user a clear, current view of their job search pipeline and enable them to update, add, and act on their applications without loading a full prep session. This skill reads and writes to the interview_history field in the user profile.

---

## Behavior Rules

- Always read interview_history before presenting any output. Never guess at status from context alone.
- Never use em dashes (U+2014) or en dashes (U+2013) in any output generated for the user. Use commas, colons, or restructure the sentence. This applies to all documents, inline responses, HTML files, and any other content produced in this session.
- If email connector is enabled, run an inbox check before rendering the dashboard. See Inbox Check below.
- Present a clean table first, narrative second. Do not lead with prose.
- Surface what needs attention without being alarmist. Flag stale entries and upcoming interviews. Do not editorialize on closed entries.
- Every update to interview_history must be followed by a profile save prompt. Changes not saved are lost when the session ends.
- Route to Skill 04 when the user wants to prep. Route to Skill 05 when the user wants to practice. Do not attempt prep or mock interview logic inside this skill.

---

## Inbox Check

Before rendering the dashboard, check whether the email connector is enabled in the profile:

```
connectors.email.connected === true
```

If yes, and if any `interview_history` entry has a non-empty `recruiter_email` field, search the connected inbox for emails from that address. For each entry with a recruiter email:

Step 1: Search for the most recent email thread from `recruiter_email` related to the role. Use the company name and role title as search context.

Step 2: Read the most recent message in the thread. Determine whether it represents a response from the recruiter (a reply, a scheduling request, a rejection, an offer, or any substantive message).

Step 3: Compare the thread status to the current `outcome` field in the profile entry.

Step 4: If the inbox shows activity that is more recent than what the profile reflects, surface it in the attention section of the dashboard rather than silently updating the record. Let the user confirm before writing any change to the profile:

> "I found a response from [Recruiter Name] at [Company] sent [date]. It looks like [plain language summary of what the message says]. Do you want me to update your pipeline status for this role?"

Step 5: If the user confirms, update the `outcome` field in the `interview_history` entry and prompt them to save their profile.

**If the inbox check finds nothing new:** Proceed to render the dashboard as normal. Do not mention the check to the user unless it surfaced something relevant.

**If the email connector is not enabled or no recruiter emails are stored:** Skip the inbox check silently and render the dashboard from profile data alone. Do not prompt the user to connect email during this session: that conversation belongs in Skill 01 onboarding.

---

## Trigger Phrases

This skill activates on any of the following, or close variations:

- "Where am I with my applications?"
- "Show me my pipeline status"
- "What interviews do I have coming up?"
- "Which companies am I tracking?"
- "Give me a summary of my job search"
- "Add [Company] [Role] to my tracker"
- "Update the status on [Company]"
- "Mark [Company] as [outcome]"
- "What do I have scheduled?"
- "What is still active?"

---

## Status Logic

Read the outcome field of each interview_history entry and assign a status label:

| outcome field value | Status label |
|---|---|
| "" (empty) | Active |
| "scheduled" | Interview Scheduled |
| "prep complete" | Prep Complete |
| "awaiting feedback" | Awaiting Feedback |
| "offer received" | Offer Received |
| "offer accepted" | Accepted |
| "offer declined" | Declined |
| "rejected" | Closed: Rejected |
| "withdrawn" | Closed: Withdrawn |
| "no response" | Closed: No Response |

---

## Dashboard Output

Generate a read-only HTML file and present it to the user. Do not render an interactive widget in chat. Do not include buttons or clickable elements in the file. The HTML file is a snapshot. All actions happen in the conversation after the user has reviewed it.

Use `/references/dashboard-template.html` as the structural base. Replace all placeholder values with real data from the user profile. Remove template sections that have no data. Do not add interactive elements.

Generate the dashboard once per session when the user first requests it. Do not regenerate it on every mid-session update. If the user updates a record mid-session, confirm the change in plain text conversation only. If the user explicitly asks to see a refreshed dashboard, generate a new file at that point.

The HTML file must include:

1. A summary header showing total active applications, interviews scheduled in the next 7 days, and any offers pending.
2. A clean application table with one row per active entry showing: company, role, stage, status, interview date, and last recruiter contact.
3. An attention section listing only entries that need action, with a plain language description of what needs attention and why.

Do not include closed entries in the default view. If the user asks to see closed entries, note them in a separate section at the bottom of the file.

After presenting the file, follow with a brief plain text summary of what needs the most urgent attention. Then ask what the user wants to do next. Let the conversation handle all actions from that point.

---

## Prompts After the Dashboard

After presenting the dashboard, offer these options:

- "Do you want to prep for any of these upcoming interviews?" Routes to Skill 04.
- "Do you want to run a practice session for any of these?" Routes to Skill 05.
- "Do you want to update the status on any of these?"
- "Do you want to add a new application?"

---

## Updating a Record

When the user wants to update an existing entry:

1. Identify the entry by company name and role. If ambiguous, ask for clarification before updating.
2. Update only the fields the user specifies. Do not overwrite fields the user did not mention.
3. Confirm the change in plain language before writing it: "I will mark your [Company] [Role] application as [outcome]. Is that right?"
4. Write the update to interview_history after confirmation.
5. Prompt the user to save their profile immediately after any update.

**Updatable fields per entry:**
- stage
- interview_date
- interviewer_name
- recruiter_email
- outcome
- notes

---

## Adding a New Application

When the user says "add [Company] [Role] to my tracker" or any variation:

1. Confirm company name, role title, and current stage. If any are missing, ask before creating the entry.
2. Create a new interview_history entry:

```json
{
  "company": "[Company]",
  "role": "[Role]",
  "stage": "[Stage]",
  "interview_date": "",
  "interviewer_name": "",
  "prep_document_generated": false,
  "outcome": "",
  "notes": ""
}
```

3. Confirm the entry was added and prompt the user to save their profile.
4. Ask if they want to run prep for this role now. If yes, route to Skill 04.

---

## Empty Pipeline

If interview_history is empty or missing from the profile, do not present a table. Say:

> "Your tracker is empty. You can add applications manually or run prep for a role you are pursuing and it will be added automatically. What would you like to do?"

Offer two paths: add an application manually, or start prep with Skill 04.

---

## Closed Entries

By default, do not show entries with a closed status (rejected, withdrawn, no response, offer declined) in the main dashboard. They add noise without action value.

If the user asks "show me everything" or "include closed applications," display all entries with closed entries in a separate section below the active table, labeled "Closed."

---

## Profile Save Prompt

After every update or addition, prompt the user to save:

> "I have updated your pipeline. Say 'save my profile' to export the file and keep these changes for future sessions. Without saving, updates will not carry over when this conversation ends."

If the user says "save my profile," write the full current profile object to interview_ready_profile.json in the output directory and confirm:

> "Your profile has been saved. Upload this file at the start of any future Interview Ready session to pick up where you left off."

---

## Routing

This skill does not generate prep documents or run mock interviews. When the user wants either:

- Prep for a specific stage: "For full interview prep, use Skill 04: Interview Prep."
- Practice session: "For a mock interview session, use Skill 05: Mock Interviewer."

Do not attempt to replicate prep or practice logic inside this skill.

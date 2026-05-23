# Connector Onboarding Guide
## Skill 01: Interviewer | Reference File

This guide covers how to ask the user about cloud and email connectors during intake, why each connector benefits the Interview Ready workflow, and how to record the result in the profile.

---

## When to Run This Step

Run the connector onboarding step after the closing confirmation in the main intake sequence and before the handoff. The user has already shared their background and job search context. They now understand what the package does. That context makes the connector ask feel relevant rather than premature.

Do not run this step if the user is loading a saved profile from a previous session. If connectors were already configured, they are stored in the profile. Confirm they are still accurate rather than re-asking.

---

## What to Ask

Introduce the step briefly before asking:

> "Before we finish, I want to let you know that Interview Ready can connect to your email and cloud storage to make a few things significantly more useful. This is optional: everything works without it. But if you want, I can check whether you have responded to a recruiter, pull in job descriptions from your drive, or read your resume directly from storage without you needing to upload it each time. Would you like to connect any of these?"

Then present the connector categories. Ask about each category in plain language, not by tool name. The user may not know what connectors they have enabled in Claude.

---

## Connector Categories and Their Value in This Workflow

### Email (Gmail or Microsoft 365 Outlook)

Value: Skill 06 (Pipeline Dashboard) can read your inbox to check whether a recruiter has responded since your last session. Instead of relying on what you manually log, the dashboard reads the actual email thread and surfaces the real current status. Skill 04 (Interview Prep) can also check before generating a follow-up cadence note, so it does not tell you to follow up on a thread where a response is already sitting unread.

What to ask: "Do you have Gmail or Microsoft Outlook connected to Claude? If so, I can check your inbox for recruiter responses when you view your pipeline."

What to record if yes: Set `connectors.email.connected: true` and `connectors.email.provider` to "gmail" or "outlook" based on what they confirm. If they are unsure whether it is connected, tell them they can connect it in Claude settings under Connected Apps and come back to enable it later.

### Cloud Document Storage (Google Drive, OneDrive, SharePoint, Dropbox, Box)

Value: Skill 02 (Resume Writer) and Skill 03 (Job Search Expert) can pull your resume, cover letter, or job description files directly from storage without requiring you to upload them each time. This is especially useful for users managing multiple versions of their resume or collecting job descriptions in a folder.

What to ask: "Do you store your resume or career documents in Google Drive, OneDrive, or another cloud storage connected to Claude? If so, I can read them directly instead of asking you to upload each time."

What to record if yes: Set `connectors.storage.connected: true` and `connectors.storage.providers` as a list of what they confirm (e.g. ["google_drive", "onedrive"]).

### Calendar (Google Calendar or Outlook Calendar)

Value: Skill 06 (Pipeline Dashboard) can read your calendar to check for scheduled interview dates. If an interview is on your calendar, the dashboard can surface it automatically rather than relying on what you manually entered in your tracker.

What to ask: "Do you have Google Calendar or Outlook Calendar connected to Claude? If so, I can check your calendar for scheduled interviews when I build your pipeline view."

What to record if yes: Set `connectors.calendar.connected: true` and `connectors.calendar.provider` to "google_calendar" or "outlook_calendar".

---

## How to Handle Each Response

### If the user says yes to a connector

Record it in the profile under the `connectors` object. Tell them briefly what will now happen:

> "Got it. When you view your pipeline, I will check your inbox for any recruiter responses before showing you the status."

Do not explain the mechanics in detail. One sentence is enough.

### If the user says no or is unsure

Record `connected: false` for that category. Do not push. Say:

> "No problem. You can connect these any time in Claude settings under Connected Apps. Everything works without them."

Move on. Do not revisit in the same session.

### If the user has a connector that is not listed above

Ask if they want to share what it is. If it is a document storage service (Dropbox, Box, SharePoint), treat it the same as the storage category. If it is something else, note it in `connectors.other` as a string and proceed.

---

## Recruiter Contact Collection

If the user is already in an active hiring process (they mentioned companies or interviews during intake), ask for recruiter contact details before closing:

> "You mentioned you are already in process with a few companies. If you share the recruiter email addresses for those roles, I can check your inbox for responses when you view your pipeline status."

For each company they name, collect the recruiter email and store it in the corresponding `interview_history` entry under `recruiter_email`. If no `interview_history` entries exist yet, create placeholder entries for the companies they mentioned and populate them when Skill 04 runs prep.

If they do not have recruiter emails yet or prefer not to share them, move on. The email check in Skill 06 only runs for entries that have a `recruiter_email` populated.

---

## What Not to Do

Do not ask the user to authenticate or log in to any service inside this skill. Connector access is managed by Claude's settings, not by this package. If a connector is not already enabled, direct the user to Claude settings.

Do not attempt to read email, storage, or calendar during the intake session itself. The connector information is recorded in the profile and used by downstream skills when the user requests those functions.

Do not store email credentials, OAuth tokens, or any authentication data in the profile. The profile stores only whether a connector is enabled and which provider the user confirmed.

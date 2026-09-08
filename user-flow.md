# User flow

This document explains exactly what the user does in ChatGPT and what ChatGPT automates.

## Responsibility legend

- **USER**: action the user must perform manually
- **GPT**: action ChatGPT performs during setup or scheduled runs
- **USER + GPT**: ChatGPT presents one choice or question, and the user approves or answers it

## One-time setup

### Step 1 - USER - Connect Gmail and Google Drive

Where:

`ChatGPT > Settings > Apps`

or, depending on the account interface:

`ChatGPT > Settings > Plugins`

Connect the Google account that receives the user's job-alert emails and owns the Google Drive resources for this workflow.

This step is required because the workflow needs Gmail read access and Google Drive read access. Tracker automation also needs supported Google Drive write actions.

### Step 2 - USER - Start the bootstrap prompt

Where:

A normal ChatGPT conversation.

How:

1. Open `prompts/01-bootstrap.md` in this GitHub repository.
2. Copy the complete prompt.
3. Start a new ChatGPT conversation.
4. Paste the prompt and send it.

No local program needs to be installed or executed.

### Step 3 - USER + GPT - Complete conversational onboarding

ChatGPT asks the onboarding questions directly in the same conversation.

The user does not open a separate questionnaire.

The onboarding is intentionally sequential:

- ChatGPT asks exactly one question per turn.
- Each question is marked `Required` or `Optional`.
- Optional questions can be skipped.
- Unfamiliar concepts are explained in plain language.
- Technical internal terms are not used without explanation.
- Recommended defaults are used for technical settings whenever possible.

Examples of user-facing wording:

Instead of:

`What is your target seniority?`

ChatGPT asks:

`What level of role are you looking for? For example: Junior, Mid-level, Senior, Staff, Lead, Manager, or Director.`

Instead of:

`What adjacent titles should be included?`

ChatGPT asks:

`Are there similar job titles you would also consider? These are not your first choice, but close enough that you would still want to see them.`

People management is explained separately from leadership.

People management means direct reports and responsibilities such as 1:1 meetings, performance reviews, hiring, or managing team members.

Project leadership, mentoring, design direction, or cross-functional leadership without direct reports is collected separately and does not count as people management.

The questions cover:

- main target role
- similar roles the user would also consider
- desired job level
- roles or levels to always exclude
- direct people-management preference
- current or recent career evidence
- relevant experience
- strongest skills and responsibilities
- measurable outcomes when available
- leadership experience without direct reports
- preferred and excluded domains
- target locations and work model
- employment type
- work authorization and sponsorship handling
- compensation preferences when relevant
- optional company and keyword preferences
- optional existing application history
- automation preference
- Gmail job-alert sources
- daily run time and timezone

The email scan window uses the previous local calendar day by default, so the user does not need to configure a technical time window unless they want something different.

### Step 4 - GPT - Create private profile and a new Tracker

ChatGPT creates and stores the resources itself.

The user does not manually create or move generated files.

Created resources:

1. private career profile
2. new Google Sheet tracker

Preferred profile:

`Job_Mail_Collector_Profile.md`

Compatibility fallback:

A Google Doc containing the exact Markdown content.

Default Sheet:

`Job_Mail_Collector`

Tabs:

- Config
- Sources
- Tracker
- Control

A new Tracker is created by default for a new setup.

The workflow does not require the user to already have a tracker.

If the user says they already have past application history and wants it imported, ChatGPT may locate that source only after explicit authorization and import compatible history into the new Job Mail Collector Tracker.

ChatGPT must not search Drive for arbitrary existing trackers before the user opts into import.

ChatGPT must not silently reuse a similarly named existing spreadsheet.

### Step 5 - USER + GPT - Create the Scheduled Task

The schedule questions are asked near the end of onboarding.

The user selects a daily run time and confirms a timezone in plain language. A city name is acceptable when ChatGPT can normalize it to the correct timezone.

ChatGPT then creates the recurring Scheduled Task.

The user does not launch a program at that time. The task runs inside ChatGPT.

After creation, the task can be reviewed or edited from ChatGPT's `Scheduled` page.

If task creation requires an additional confirmation in the user's interface, the user completes that confirmation.

### Step 6 - GPT - Validate the setup

Before setup is considered complete, ChatGPT performs a test run and checks:

- profile access
- Gmail access
- enabled job-alert sources
- digest parsing
- Sheet access
- Tracker read completeness
- application-link extraction
- Tracker write capability when automatic writing is enabled

## Daily scheduled workflow

### Step 7 - GPT - Read Gmail and identify jobs

At the configured time, the Scheduled Task runs inside ChatGPT.

ChatGPT:

1. loads the user's current private profile;
2. reads job-alert emails in the configured time window;
3. expands digest emails into individual jobs;
4. extracts job data and application links;
5. removes duplicates and hard mismatches;
6. classifies the remaining jobs by fit.

The user does not manually trigger this daily scan.

### Step 8 - GPT - Update the Tracker automatically

When authorized Google Drive write actions are available, ChatGPT appends suitable new jobs to the Tracker as `Candidate`.

If an automatic scheduled write cannot proceed because approval is required or the write action is unavailable, ChatGPT returns the exact rows as TSV instead and reports the write limitation.

This TSV path is a fallback, not the preferred primary workflow.

### Step 9 - GPT - Deliver recommended jobs

The Scheduled Task result contains a readable shortlist with application links.

For each recommended job, ChatGPT should show enough information for the user to decide whether to apply.

Typical fields:

- company
- role
- location
- work model
- salary when available
- match level
- short reason
- source
- application link

### Step 10 - USER - Apply to jobs

The user opens a recommended application link and completes the application on the external site.

This is the main manual action in the daily workflow.

Job Mail Collector does not claim to submit applications on the user's behalf.

### Step 11 - GPT - Reconcile application and response status

On later scheduled runs, ChatGPT scans relevant Gmail evidence and updates the Tracker when the evidence is clear.

Automatic examples:

- clear application confirmation -> `Status = Applied`, fill `AppliedAt`
- clear employer or recruiter reply -> fill `RespondedAt`
- explicit rejection -> `Status = Closed`, fill `Result`, optionally fill `RejectionStage`
- recognizable ATS sender domain -> fill `ATS` when enabled

Do not update state from weak evidence.

If the user applied but no confirmation email exists, ChatGPT may not know that the application happened. The user can manually change that Tracker row to `Applied`.

### Step 12 - GPT - Monitor no-response cases and anomalies

When enabled, ChatGPT identifies applications that have had no response for the configured number of days.

It also reports:

- broken or missing links
- source channels with zero messages
- profile read failures
- incomplete Tracker reads
- ambiguous responses
- failed Tracker writes

No-response rows should not be automatically closed unless the user has explicitly chosen that behavior.

## Simplified flow

```text
USER
Connect Gmail + Google Drive in ChatGPT
        |
        v
USER
Paste bootstrap prompt into a new ChatGPT chat
        |
        v
USER + GPT
GPT asks ONE plain-language question at a time
Required/Optional is shown for every question
        |
        v
GPT
Create private profile + NEW Tracker in Google Drive
        |
        +---- optional: import existing history only if user requests it
        |
        v
GPT
Create and test Scheduled Task
        |
        v
================ DAILY ================
        |
        v
GPT
Read job-alert emails
        |
        v
GPT
Filter + match + deduplicate + extract apply links
        |
        v
GPT
Write suitable jobs to Tracker when permitted
        |
        v
GPT
Send shortlist with application links
        |
        v
USER
Open links and apply
        |
        v
GPT
Detect confirmations/replies and update Tracker later
```

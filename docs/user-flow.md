# User flow

This document explains exactly what the user does in ChatGPT and what ChatGPT automates.

## Responsibility legend

- **USER**: action the user must perform manually
- **GPT**: action ChatGPT performs during setup or scheduled runs
- **USER + GPT**: ChatGPT presents one choice or question, and the user approves, answers, or corrects it

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

The onboarding is a conversation, not a form.

At the beginning, ChatGPT tells the user that they can answer naturally and do not need to provide one exact value per question.

For example, the user can write:

> I am mainly looking for Senior Product Designer roles, but UX/UI or Lead roles are also interesting when the work fits. I have about nine years of B2C product design experience and a lot of design-system work.

ChatGPT should extract every useful piece of information from that answer, organize it internally, and skip later questions that have already been answered.

The user does not need to know terms such as `target_seniority`, `adjacent_titles`, YAML, or internal matching fields.

#### Phase 1: understand the user first

ChatGPT first collects broad factual evidence about:

- what kind of work the user is looking for;
- current or recent work;
- relevant experience;
- products, industries, and business context;
- strongest responsibilities and skills;
- measurable outcomes when available;
- leadership and people-management evidence when relevant.

ChatGPT asks exactly one question per turn, but a single user answer may resolve several topics at once.

`Required` means the topic eventually needs to be understood for reliable matching. It does not mean the user must enter a rigid or exact-format value.

Optional questions can be skipped or answered with `none`, `not sure`, or natural-language equivalents.

#### Phase 2: infer the search scope

After enough evidence is collected, ChatGPT summarizes how it currently understands the user's search.

The matching model is fit-based rather than title-gated.

For example, a user mainly targeting `Senior Product Designer` does not need to decide in advance whether every `Staff` or `Lead` role is allowed.

If the actual responsibilities and required scope fit the user's experience, those jobs can still be considered.

ChatGPT treats:

- the main title or direction as a **core target**;
- nearby titles or broader levels as **consider if fit** when supported by evidence;
- only explicit unwanted roles or constraints as **hard exclusions**.

The user is shown a short interpretation summary and can correct it in normal language.

Example correction:

> Lead is fine, but I do not want Manager roles yet. UX/UI is fine only for digital products.

ChatGPT should update all affected matching rules from that one answer.

#### Phase 3: ask only meaningful constraints

After career evidence and search direction are understood, ChatGPT asks only for unresolved conditions that could materially change recommendations, such as:

- target location;
- remote, hybrid, or on-site constraints;
- employment type;
- work authorization and sponsorship handling;
- explicit unwanted roles or domains;
- optional compensation limits;
- optional company or keyword preferences.

It should prefer asking what the user definitely does not want over making the user enumerate every title or career level they would accept.

People management is kept separate from leadership.

People management means direct reports and responsibilities such as 1:1 meetings, performance reviews, hiring, or managing team members.

Project leadership, mentoring, design direction, process leadership, or cross-functional influence without direct reports does not count as people management.

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

The private profile stores both structured fields and natural-language interpretation notes.

The role interpretation should preserve nuance such as:

- `Senior is the main target, but Staff or Lead roles should still be considered when the actual scope fits.`
- `Manager roles are excluded because the user does not want direct people management.`

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

### Step 5 - USER + GPT - Configure automation and schedule

ChatGPT asks whether the user wants the recommended automation settings.

The recommended configuration includes Tracker writes, application-confirmation detection, response detection, rejection handling when evidence exists, ATS estimation when recognizable, and no-response checks after 14 days.

If the user wants customization, only the settings they want to change should be discussed, one question at a time.

Schedule questions are asked near the end of onboarding.

The user selects a daily run time and confirms a timezone in plain language. A city name is acceptable when ChatGPT can normalize it to the correct timezone.

ChatGPT then creates the recurring Scheduled Task.

The user does not launch a program at that time. The task runs inside ChatGPT.

If task creation requires an additional confirmation in the user's interface, the user completes that confirmation.

### Step 6 - GPT - Validate the setup

Before setup is considered complete, ChatGPT performs a test run and checks:

- profile access;
- Gmail access;
- enabled job-alert sources;
- digest parsing;
- Sheet access;
- Tracker read completeness;
- application-link extraction;
- Tracker write capability when automatic writing is enabled.

## Daily scheduled workflow

### Step 7 - GPT - Read Gmail and identify jobs

At the configured time, the Scheduled Task runs inside ChatGPT.

ChatGPT:

1. loads the user's current private profile;
2. reads job-alert emails in the configured time window;
3. expands digest emails into individual jobs;
4. extracts job data and application links;
5. removes duplicates and explicit hard mismatches;
6. evaluates remaining jobs against the user's actual career evidence and constraints.

The user does not manually trigger this daily scan.

### Step 8 - GPT - Judge fit by actual scope, not title alone

The daily workflow does not use title or career-level labels as automatic gates unless the user explicitly configured them as hard exclusions.

For each role, ChatGPT compares:

- actual responsibilities;
- required experience;
- ownership and decision-making scope;
- leadership or people-management expectations;
- relevant skills and evidence;
- domain context;
- location, work model, employment, and authorization constraints.

A `Staff` or `Lead` role can still be recommended to a user mainly targeting `Senior` when the actual scope is supported by the profile.

A superficially similar title can still be downgraded when the actual responsibilities are materially unsupported.

### Step 9 - GPT - Update the Tracker automatically

When authorized Google Drive write actions are available, ChatGPT appends suitable new jobs to the Tracker as `Candidate`.

If an automatic scheduled write cannot proceed because approval is required or the write action is unavailable, ChatGPT returns the exact rows as TSV instead and reports the write limitation.

This TSV path is a fallback, not the preferred primary workflow.

### Step 10 - GPT - Deliver recommended jobs

The Scheduled Task result contains a readable shortlist with application links.

For each recommended job, ChatGPT should show enough information for the user to decide whether to apply.

Typical fields:

- company;
- role;
- location;
- work model;
- salary when available;
- match level;
- short evidence-based reason;
- source;
- application link.

A role that is a reasonable stretch can still appear when the evidence supports it. The explanation should make the scope difference clear rather than rejecting it only because of the title.

### Step 11 - USER - Apply to jobs

The user opens a recommended application link and completes the application on the external site.

This is the main manual action in the daily workflow.

Job Mail Collector does not claim to submit applications on the user's behalf.

### Step 12 - GPT - Reconcile application and response status

On later scheduled runs, ChatGPT scans relevant Gmail evidence and updates the Tracker when the evidence is clear.

Automatic examples:

- clear application confirmation -> `Status = Applied`, fill `AppliedAt`;
- clear employer or recruiter reply -> fill `RespondedAt`;
- explicit rejection -> `Status = Closed`, fill `Result`, optionally fill `RejectionStage`;
- recognizable ATS sender domain -> fill `ATS` when enabled.

Do not update state from weak evidence.

If the user applied but no confirmation email exists, ChatGPT may not know that the application happened. The user can manually change that Tracker row to `Applied`.

### Step 13 - GPT - Monitor no-response cases and anomalies

When enabled, ChatGPT identifies applications that have had no response for the configured number of days.

It also reports:

- broken or missing links;
- source channels with zero messages;
- profile read failures;
- incomplete Tracker reads;
- ambiguous responses;
- failed Tracker writes.

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
User answers naturally
GPT asks one question at a time and extracts multiple facts when possible
        |
        v
GPT
Build a fit-based search interpretation from career evidence
        |
        v
USER + GPT
User corrects or narrows only what matters
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
Filter hard exclusions + evaluate actual job scope + deduplicate
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

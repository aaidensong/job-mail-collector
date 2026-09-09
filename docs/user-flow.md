# User flow

This document explains what the user does in ChatGPT and what ChatGPT automates.

## Responsibility legend

- **USER**: action the user performs manually
- **GPT**: action ChatGPT performs during setup or scheduled runs
- **USER + GPT**: ChatGPT asks, proposes, or requests approval and the user responds

## One-time setup

### Step 1 - USER - Connect Gmail and Google Drive

In ChatGPT, open `Settings > Apps` or `Settings > Plugins`, depending on the account interface.

Connect the Google account that receives job-alert emails and owns the Drive resources for this workflow.

### Step 2 - USER - Start the bootstrap prompt

1. Open `prompts/01-bootstrap.md`.
2. Copy the complete prompt.
3. Start a new normal ChatGPT conversation.
4. Paste and send the prompt.

No local application, script, terminal command, or server is required.

### Step 3 - USER + GPT - Complete conversational onboarding

The onboarding is a conversation, not a form.

The user can answer naturally and may mention several pieces of information in one message.

Example:

> I am mainly looking for Senior Product Designer roles, but UX/UI, Growth, or Lead roles are also interesting when the work fits. I have about nine years of B2C product design experience and a lot of design-system and funnel work.

ChatGPT extracts all useful facts from that answer and skips questions already resolved.

Normal questions are not labeled `Required` or `필수`.
Only optional questions are marked. In Korean, optional questions use:

`선택 질문입니다. 필요하지 않다면 답변하지 않으셔도 됩니다.`

Examples are shown when useful, but users do not need to copy the format.

#### Understand the person before narrowing the search

ChatGPT first gathers enough evidence about:

- the kind of work the user wants
- current or recent role
- relevant experience
- actual ownership and scope
- products, industries, and problem areas
- strongest skills and responsibilities
- recurring problem types
- measurable or observable impact
- decision-making under ambiguity
- cross-functional influence
- leadership or mentoring
- specialty or differentiating expertise

For experienced users, a title such as `Senior Product Designer` is not considered sufficient evidence by itself. ChatGPT asks role-specific depth questions when needed to understand what actually distinguishes the person.

#### Infer search scope instead of forcing a whitelist

ChatGPT treats:

- main role direction as **Core target**
- nearby titles or broader levels as **Consider if fit**
- explicitly unwanted roles or constraints as **Hard exclude**

A user mainly targeting `Senior Product Designer` does not need to decide in advance whether every Staff or Lead role is allowed. If the actual responsibilities and scope fit the user's evidence, those jobs can still be considered.

ChatGPT then shows a concise interpretation summary. The user can correct or narrow it in normal language.

#### Ask only material constraints

After the person's career evidence is understood, ChatGPT asks only unresolved conditions that could materially change recommendations, such as location, work model, employment type, work authorization, explicit exclusions, or compensation limits.

Resume-version tracking is not part of the core workflow.

### Step 4 - GPT - Create private profile and new Tracker

ChatGPT creates:

1. a private career profile
2. a new Google Sheet Tracker

Preferred profile:
`Job_Mail_Collector_Profile.md`

Fallback:
a private Google Doc containing the same Markdown text.

Default Sheet:
`Job_Mail_Collector`

Tabs:
- Config
- Sources
- Tracker
- Control

The Tracker has exactly 13 columns:

`Status, Company, Title, Location, Salary, WorkMode, Notes, Link, ReceivedAt, AppliedAt, RespondedAt, Result, Source`

There are no ATS, ResumeVersion, Channel, or RejectionStage columns.

`Source` is the single provenance field. If recruiter or agency context is important for a row, it goes in Notes.

A new Tracker is created by default. Existing application history is imported only when the user explicitly requests it.

### Step 5 - USER + GPT - Configure automation and schedule

Recommended automation includes:

- add suitable jobs to Tracker
- detect clear application confirmations
- detect explicit recruiter submission evidence
- reconcile clear application status changes
- detect employer or recruiter responses
- flag no-response applications after 14 days

ATS and rejection-stage inference are not part of the core workflow.

The user chooses a daily run time and timezone in plain language. A city name is acceptable when ChatGPT can normalize it.

ChatGPT creates the recurring Scheduled Task.

### Step 6 - GPT - Validate setup

The setup test checks profile access, Gmail access, source parsing, digest expansion, Tracker completeness, 13-column output, application-link extraction, write capability, fit-based matching, and missed-run recovery logic.

## Daily scheduled workflow

### Step 7 - GPT - Calculate the unprocessed date range

The task uses `Control.last_successful_scan_date`.

- First run: process the previous local calendar day.
- Later runs: process every calendar day after the last successful date through yesterday.
- If a scheduled run was skipped or failed, the next successful run catches up automatically.

The task shows the scan period at the top of the result.

The success marker is updated only after the full target period was processed successfully enough to produce normal output or a complete TSV write fallback.

### Step 8 - GPT - Read and classify Gmail messages

Candidate collection searches the confirmed Sources for the target period.

For reconciliation, ChatGPT can also read target-period inbox messages in one broad pass.

Messages are classified using sender + subject + body, not sender address alone.

Useful categories include:

- job alert
- application confirmation
- recruiter submission evidence
- employer or recruiter response
- marketing/newsletter
- unknown

One sender may produce several message types.

### Step 9 - GPT - Extract, normalize, and deduplicate jobs

ChatGPT expands digest messages, extracts individual jobs, preserves valid links, normalizes stable LinkedIn links when a job ID is explicit, and never invents missing URLs.

Within-run duplicates use Company + Title, with location as a tie-breaker when needed.

If the same job appears from multiple sources, Source can contain a combined value such as `Glassdoor + LinkedIn`.

If company identity is ambiguous, ChatGPT does not guess the parent company. Suspected duplicates go to Human review rather than being merged automatically.

### Step 10 - GPT - Evaluate actual fit

Hard exclusions are applied first.

Remaining jobs are judged using actual responsibilities, required scope, ownership, ambiguity, leadership, problem type, differentiators, domain, skills, outcomes, and practical constraints.

A Strong match for an experienced user should normally have a meaningful reason beyond title similarity.

### Step 11 - GPT - Compare with Tracker

Before absence or historical duplicate judgments, ChatGPT verifies that the number of Tracker rows read matches `Control.tracker_data_rows`.

If the read is incomplete, it does not claim that a row is absent and skips history-dependent reconciliation.

### Step 12 - GPT - Reconcile applications and responses in one inbox pass

When enabled, ChatGPT reads target-period inbox messages once and compares likely application confirmations, recruiter submissions, and employer/recruiter responses against relevant Tracker rows.

It does not run a separate Gmail search for every Applied company by default.

A targeted follow-up search is used only when a specific ambiguity needs resolution.

Clear application evidence can include an explicit recruiter statement that a profile, resume, or application was submitted for a specific company and role.
Ambiguous future-intent language goes to Human review.

### Step 13 - GPT - Update Tracker

When writes are permitted:

- suitable jobs are added as Candidate
- clear application evidence can set Status=Applied and AppliedAt
- clear replies can fill RespondedAt
- explicit rejection can set Status=Closed and Result=Rejected
- other explicit final outcomes can be stored in Result

No ATS or rejection-stage field is stored.

If an automatic write is blocked, ChatGPT returns exact 13-column TSV as a fallback.

### Step 14 - USER - Apply to jobs

The user opens recommended links and completes applications on external sites.

Job Mail Collector does not claim to submit applications on the user's behalf.

### Step 15 - GPT - Monitor no-response cases and source health

When enabled, ChatGPT identifies Applied rows with no response after the configured number of days.

It also reports enabled job-alert sources with zero messages and warns when all major sources unexpectedly return zero messages.

No-response rows are not automatically closed by default.

## Simplified flow

```text
USER
Connect Gmail + Google Drive
        |
        v
USER
Paste bootstrap prompt into ChatGPT
        |
        v
USER + GPT
Natural-language onboarding
        |
        v
GPT
Build fit-based profile with real differentiators
        |
        v
GPT
Create private profile + 13-column Tracker
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
Calculate catch-up scan period
        |
        v
GPT
Read alerts + classify messages
        |
        v
GPT
Extract + deduplicate + evaluate actual fit
        |
        v
GPT
Single-pass application/response reconciliation
        |
        v
GPT
Write Tracker updates when permitted
        |
        v
GPT
Return shortlist + source health + diagnostics
        |
        v
USER
Open links and apply
```
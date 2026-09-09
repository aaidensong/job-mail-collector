# Architecture

## 1. ChatGPT setup layer

Runs once in a normal ChatGPT conversation after the user connects Gmail and Google Drive.

User responsibilities:
- connect Gmail and Google Drive
- paste the bootstrap prompt into a new ChatGPT chat
- answer onboarding questions naturally
- approve product permission or task-creation prompts when required

GPT responsibilities:
- verify app access
- show setup progress and an approximate remaining-answer range
- collect career evidence and matching constraints conversationally
- create the private profile
- discover and confirm Gmail alert sources
- create a brand-new Tracker Sheet using the workflow's own schema
- verify read and write behavior
- create the Scheduled Task
- run a setup test

The user should not manually create, move, or wire together the profile and Tracker resources.

### Progress-visible onboarding

The setup conversation has four user-facing stages:

1. Career direction & evidence
2. Search constraints
3. Automation & job-alert sources
4. Schedule, create, and test

Most setups are expected to take about 8-12 user answers when recommended settings are used, but the estimate is dynamic. One detailed response can resolve multiple topics and remove later questions.

Before each direct onboarding question, ChatGPT shows a compact stage and remaining-answer estimate. Permission dialogs and approval taps are not counted as onboarding answers.

### Fresh Tracker boundary

Bootstrap always creates a new Job Mail Collector Tracker.

It does not:
- ask whether the user already has a tracker;
- search Drive for an existing tracker;
- import historical spreadsheet data;
- adapt the workflow to an arbitrary existing schema;
- reuse an existing `Job_Mail_Collector` file.

If the preferred name already exists, the setup creates a uniquely named new Sheet automatically.

Historical spreadsheet migration, if desired, is a separate post-setup task that should be handled in another ChatGPT conversation.

This boundary does not affect ongoing Gmail reconciliation. A scheduled run may still create a missing Applied row from clear application-confirmation or recruiter-submission evidence.

## 2. Private profile layer

A private Markdown-formatted document owned by the user.

Preferred name:
`Job_Mail_Collector_Profile.md`

The profile stores:
- main target direction and nearby roles to consider when actual fit is strong
- career background and relevant experience
- differentiators, scope, impact, and recurring problem types
- strengths and skills
- domain preferences and exclusions
- geography and work model
- employment constraints
- work authorization and sponsorship handling
- hard exclusions and warnings
- leadership evidence kept separate from direct people management

The profile uses `profile_version = 2`.
It does not track resume versions.

Public examples must be fictional and must not be derived from a user's private profile or prior conversation history.

## 3. Operational configuration layer

Google Sheet owned by the user.

### Config
Stores schedule, profile reference, automation settings, and write behavior.

Current schema uses `config_version = 4`.

### Sources
Stores confirmed Gmail sender patterns plus source-specific parsing and message-classification notes.

A sender may produce more than one message type. Classification uses sender + subject + body.

### Tracker
Stores job candidates and application history in 13 columns:

`Status, Company, Title, Location, Salary, WorkMode, Notes, Link, ReceivedAt, AppliedAt, RespondedAt, Result, Source`

The Tracker intentionally does not store ATS, ResumeVersion, Channel, or RejectionStage.

`Source` is the single provenance field. Agency or recruiter details go in Notes only when useful.

### Control
Stores verification metrics and `last_successful_scan_date`.

`last_successful_scan_date` lets the workflow catch up automatically after a skipped or failed scheduled run.

## 4. Scheduled ingestion layer

Runs automatically inside ChatGPT at the configured time.

Responsibilities:
- load Config and Control
- calculate every unprocessed local calendar day through yesterday
- read and validate the private profile
- read configured Gmail job-alert sources
- expand digest messages
- classify messages by actual content
- extract structured jobs
- normalize links
- deduplicate within the run

The scan marker advances only after the target period is fully processed successfully enough to produce normal output or a complete TSV write fallback.

## 5. Matching layer

Two-stage decision model:

1. Hard filters remove explicit non-starters.
2. Fit matching classifies the remainder as Strong, Possible, or Weak.

Matching prioritizes actual responsibilities and demonstrated career evidence over exact title equality.

For experienced users, Strong matches should normally include a meaningful reason beyond title similarity, such as matching problem type, ownership scope, measurable impact, specialty, domain depth, or leadership evidence.

## 6. History and completeness layer

Before making absence or historical duplicate judgments, the workflow compares rows actually read from Tracker with `Control.tracker_data_rows`.

When completeness is not verified:
- absence claims are disabled
- historical duplicate confirmation is disabled
- application/response reconciliation is skipped
- candidate writes are not applied
- `last_successful_scan_date` does not advance

## 7. Single-pass inbox reconciliation layer

Candidate alert collection uses confirmed Sources.

Application and response reconciliation can use one broad Gmail read for the target period.

The workflow classifies messages into categories such as:
- job alert
- application confirmation
- recruiter submission evidence
- employer or recruiter response
- marketing/newsletter
- unknown

It then compares relevant messages against Tracker rows.

A separate Gmail query for every Applied company is avoided by default. A targeted follow-up search is used only when needed to resolve ambiguity.

Clear recruiter statements that a profile, resume, or application was submitted for a specific role can count as application evidence. Ambiguous future-intent language requires human review.

## 8. Tracker automation layer

When Tracker completeness is verified and Google Drive write actions are available, the task can:
- append suitable jobs as `Candidate`
- reconcile clear application evidence to `Applied`
- fill `AppliedAt`
- fill `RespondedAt` after clear employer or recruiter responses
- close explicitly rejected applications with `Result = Rejected`
- record other explicit final outcomes in Result

ATS and rejection-stage inference are intentionally excluded from the core workflow because they add complexity without enough user value.

Ambiguous evidence is not written automatically.

If a scheduled external write cannot proceed because approval is required or a write action is unavailable, the workflow returns exact 13-column TSV as a fallback.

## 9. User application layer

The user reviews recommended links and submits applications on external sites.

Application submission is intentionally outside the core workflow because employers and application systems use different forms, authentication, consent, and submission requirements.

If an application generates no confirmation and no clear recruiter-submission evidence, the user may need to mark the row Applied manually.

## 10. Monitoring and delivery layer

The Scheduled Task result provides:
- scan period
- readable shortlist with reasons and application links
- detected applications and responses
- no-response candidates
- Tracker write results
- human-review items
- source-health checks
- diagnostics
- TSV only when automatic Tracker writing could not be applied

Enabled alert sources with zero messages are always surfaced. If all major configured sources unexpectedly return zero messages, the workflow warns that alert delivery, sender patterns, or account configuration may need review.

## Data flow

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
Conversational onboarding with visible progress
        |
        v
GPT
Create private profile + NEW Tracker + Scheduled Task
        |
        v
================ SCHEDULED RUN ================
        |
        v
Control.last_successful_scan_date -> calculate catch-up period
        |
        v
Private profile + Config + Sources + Tracker
        |
        v
Job-alert Gmail -> extraction -> hard filters -> fit matching
        |
        +------------------------------+
        |                              |
        v                              v
Candidate shortlist            Broad inbox reconciliation
                                       |
                                       v
                           applications + responses
        |                              |
        +---------------+--------------+
                        |
                        v
              Tracker updates when permitted
                        |
                        v
              Result + source health + diagnostics
                        |
                        v
                 USER applies externally
```
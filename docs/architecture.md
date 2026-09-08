# Architecture

## 1. ChatGPT setup layer

Runs once in a normal ChatGPT conversation after the user connects Gmail and Google Drive.

User responsibilities:
- connect Gmail and Google Drive in ChatGPT
- paste the bootstrap prompt into a new ChatGPT chat
- answer onboarding questions in that chat
- approve product permission or task-creation prompts when required

GPT responsibilities:
- verify app access
- collect career and matching information interactively
- create the private profile
- discover and confirm Gmail alert sources
- create the Tracker Sheet
- verify read and write behavior
- create the Scheduled Task
- run a setup test

The user should not manually create, move, or wire together the profile and Tracker resources.

## 2. Private profile layer

A private Markdown-formatted document owned by the user.

Preferred name:

`Job_Mail_Collector_Profile.md`

Responsibilities:
- target titles and seniority
- career background and relevant experience
- strengths and skills
- domain preferences and exclusions
- geography and work model
- employment constraints
- work authorization and sponsorship handling
- hard exclusions and warnings
- richer interpretation notes

The document uses YAML front matter for stable machine-readable fields and Markdown sections for richer career evidence.

When raw Markdown storage is unavailable, a Google Doc containing the exact Markdown text is the compatibility fallback.

## 3. Operational configuration layer

Google Sheet owned by the user.

### Config
Stores schedule, profile reference, scan window, module settings, and Tracker write behavior.

### Sources
Stores confirmed Gmail sender patterns plus source-specific parsing notes.

### Tracker
Stores job candidates and application history.

### Control
Stores verification metrics used to detect incomplete Tracker reads.

## 4. Scheduled ingestion layer

Runs automatically inside ChatGPT at the configured time.

Responsibilities:
- read Config
- read and validate the private profile
- determine the local scan window
- read configured Gmail sources
- expand digest emails
- extract structured jobs
- normalize links
- deduplicate within the run

The user does not manually start a local program for each run.

## 5. Matching layer

Two-stage decision model:

1. Hard filters remove explicit non-starters.
2. Soft matching classifies the remainder as Strong, Possible, or Weak.

This prevents an explicitly disallowed role from surviving only because other attributes match well.

## 6. Tracker automation layer

When Tracker completeness is verified and Google Drive write actions are available, the task can automatically:
- append suitable new jobs as `Candidate`
- reconcile clear application-confirmation emails to `Applied`
- fill `AppliedAt`
- fill `RespondedAt` after clear employer or recruiter responses
- close explicitly rejected applications
- record rejection stage when enabled and supported by evidence
- record recognizable ATS platforms when enabled

Ambiguous evidence is not written automatically.

If a scheduled external write cannot proceed because approval is required or a write action is unavailable, the workflow returns the intended changes as TSV. This is a fallback path, not the preferred primary workflow.

## 7. User application layer

The user reviews the recommended links and submits applications on the relevant external sites.

Application submission is intentionally outside the core workflow because different employers and ATS systems have different forms, authentication, consent, and submission requirements.

After the user applies, later Gmail evidence can be used to reconcile Tracker state automatically.

If an application generates no confirmation email, the user may need to mark the row `Applied` manually because the workflow otherwise has no reliable evidence that submission occurred.

## 8. History and monitoring layer

When the Tracker read is verified as complete, the task can:
- suppress historical duplicates
- surface prior-company context
- detect untracked applications
- detect employer or recruiter responses
- find no-response cases

When completeness is not verified, absence-based judgments are disabled.

## 9. Delivery layer

The Scheduled Task result provides:
- readable shortlist with reasons and application links
- automatic Tracker write results
- human-review items
- diagnostics
- TSV only when automatic Tracker writing could not be applied

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
Interactive onboarding
        |
        v
GPT
Create profile + Tracker + Scheduled Task
        |
        v
================ SCHEDULED RUN ================
        |
        v
Private profile -------------------------------+
                                              |
Config ---------------------------------------+
                                              |
Gmail job alerts                              |
        |                                     |
        v                                     v
Email extraction -> normalization -> hard filters -> fit matching
                                                |
Sources ----------------------------------------+
Tracker -> dedupe + status reconciliation ------+
Control -> read completeness -------------------+
                                                |
                                                v
GPT -> write candidates/status to Tracker when permitted
                                                |
                                                v
GPT -> return shortlist + application links + diagnostics
                                                |
                                                v
USER -> apply on external site
                                                |
                                                v
Gmail confirmations/replies -> next scheduled reconciliation
```

## Core vs optional behavior

### Core
- ChatGPT onboarding
- scheduled Gmail scan
- private profile read and validation
- digest expansion
- link extraction
- hard filtering
- fit classification
- within-run dedupe
- Tracker comparison when verified
- automatic Candidate write when enabled and permitted
- shortlist delivery
- diagnostics

### Optional
- missing-application reconciliation
- automatic application status updates
- recruiter or employer response detection
- no-response aging
- rejection-stage inference
- ATS inference


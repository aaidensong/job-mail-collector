# Job Mail Collector

Job Mail Collector is a reusable ChatGPT Scheduled Task workflow that reads job-alert emails from Gmail, matches postings against a private career profile, writes suitable jobs into a Google Sheet tracker when permitted, and returns application links in ChatGPT.

The public repository contains only workflow logic and templates. Each user's career profile, email data, and application history stay in that user's connected Google account.

![Job Mail Collector workflow](job-mail-collector-flow.png)

> Product behavior, available apps, and Scheduled Task capabilities can change. Last verified against OpenAI documentation: 2026-09-08.

## What the user actually does

The setup is designed so a new user only needs to do four things:

1. Connect Gmail and Google Drive to ChatGPT.
2. Copy `prompts/01-bootstrap.md` from this repository into a new ChatGPT conversation.
3. Answer the onboarding questions that ChatGPT asks in that conversation.
4. Open the job links that ChatGPT recommends and apply on the employer's site.

Everything else is handled by ChatGPT when the required permissions are available.

See `docs/user-flow.md` for the complete user and GPT responsibility map.

## Where to connect Gmail and Google Drive

In ChatGPT, open `Settings > Apps` or `Settings > Plugins`, depending on the interface available to your account. Connect the Google account that receives your job-alert emails and owns the Drive files you want Job Mail Collector to use.

After the apps are connected, the bootstrap prompt is run in a normal ChatGPT conversation. You do not run a local program, terminal command, Python script, or GitHub Action.

## One-time setup

1. Connect Gmail and Google Drive in ChatGPT.
2. Open `prompts/01-bootstrap.md` on GitHub.
3. Copy the entire prompt into a new ChatGPT conversation.
4. ChatGPT asks the profile and configuration questions directly in that chat.
5. Answer those questions in the same chat.
6. ChatGPT creates the private career profile and Google Sheet in your connected Google Drive.
7. ChatGPT creates a recurring Scheduled Task at the time you selected.
8. ChatGPT runs a validation test before setup is considered complete.

You do not manually move the generated profile or tracker files after setup. ChatGPT stores and references them in your connected Drive.

## Daily automation

At the scheduled time, ChatGPT should automatically:

1. load the user's private career profile;
2. read the configured job-alert emails from Gmail;
3. expand digest emails into individual jobs;
4. extract company, role, location, compensation, work model, source, and application links when available;
5. remove obvious mismatches and duplicates;
6. compare the remaining jobs with the user's profile;
7. write suitable new jobs to the Tracker Sheet when Google Drive write access allows it;
8. return the best matches and application links in the Scheduled Task result;
9. scan clear application confirmations and employer or recruiter responses;
10. update Tracker status fields automatically when the evidence is unambiguous;
11. report anything that requires human review.

### What is not automated

The actual job application is normally completed by the user on the employer, ATS, LinkedIn, Indeed, or other external application page.

Job Mail Collector should not claim that an application was submitted unless it has evidence such as an application-confirmation email or the user explicitly updates the Tracker.

If no application-confirmation email arrives, the user may need to mark that row as `Applied` manually.

## Automatic Tracker updates and fallback

The preferred operating mode is automatic Tracker writing.

When Google Drive write actions are available and authorized, the Scheduled Task should:

- append new suitable jobs as `Candidate`;
- change a matching row to `Applied` after a clear application-confirmation email;
- fill `RespondedAt` after a clear employer or recruiter response;
- record a clear rejection result and rejection stage when evidence supports it;
- keep ambiguous changes for human review instead of guessing.

Some ChatGPT accounts or managed workspaces can require approval before an external data change. If a scheduled write cannot proceed without approval, the task must not silently fail. It should return the intended changes as paste-ready TSV and clearly say that Tracker write approval was unavailable.

## Personal data architecture

The workflow uses two private user-owned resources.

### 1. Career profile

Preferred name:

`Job_Mail_Collector_Profile.md`

Contains:

- career background
- target titles and seniority
- relevant skills and experience
- preferred and excluded domains
- location and work-model rules
- work authorization and sponsorship handling
- company and keyword preferences
- measurable evidence used for job matching

If direct raw Markdown creation or reading is not supported, ChatGPT creates a private Google Doc containing the same Markdown text and stores that document reference in the Sheet configuration.

### 2. Tracker Sheet

Default name:

`Job_Mail_Collector`

Tabs:

- `Config`: operational settings and automation modes
- `Sources`: confirmed Gmail alert sources
- `Tracker`: job candidates and application history
- `Control`: read-completeness verification

The career profile is the source of truth for matching. The Sheet does not duplicate the user's full career history.

## Core capabilities

- interactive one-time onboarding in ChatGPT
- private career profile creation
- Gmail job-alert source discovery and confirmation
- Google Sheet tracker creation
- recurring Scheduled Task creation
- digest-email expansion
- hard filtering and soft matching
- application-link extraction without invented URLs
- current-run and historical deduplication
- automatic Tracker writes when authorized
- application-confirmation detection
- employer and recruiter response detection
- no-response aging checks
- rejection-stage estimation when evidence exists
- ATS estimation from recognizable sender domains
- diagnostics for incomplete reads, missing permissions, parsing failures, and ambiguous evidence

## Repository structure

```text
job-mail-collector/
├── LICENSE
├── README.md
├── assets/
│   └── job-mail-collector-flow.png
├── prompts/
│   ├── 01-bootstrap.md
│   ├── 02-daily-job-mail-collector.md
│   ├── 03-test-run.md
│   └── 04-update-profile.md
├── profiles/
│   └── profile.template.md
├── docs/
│   ├── user-flow.md
│   ├── architecture.md
│   ├── profile-file.md
│   ├── profile-questionnaire.md
│   ├── sheet-schema.md
│   ├── matching-rules.md
│   └── troubleshooting.md
└── examples/
    ├── profile.example.md
    ├── sources.example.tsv
    └── output.example.md
```

## Design principles

- **ChatGPT-first onboarding.** A new user should not need to understand the internal schema before setup.
- **User and GPT responsibilities are explicit.** The user connects apps, answers questions, and applies to jobs. GPT handles ingestion, matching, tracking, and monitoring.
- **Private profile outside the public prompt.** Career data lives in a user-owned Drive resource.
- **No fabricated links.** Missing or unreadable application URLs stay blank.
- **Hard filters before matching.** Clear exclusions should not be rescued by a strong score elsewhere.
- **Evidence with every judgment.** A match, exclusion, or status update must be explainable from the email, posting, profile, or Tracker.
- **No absence claims from partial data.** Tracker-dependent judgments stop when the Tracker read cannot be verified as complete.
- **Automatic writes with a safe fallback.** Write to the Tracker when authorized. Return TSV when scheduled writes cannot proceed.
- **No invented application state.** Application and response status changes require clear evidence.
- **Configuration outside the task prompt.** Users can update profile and operational settings without rewriting the daily logic.

## Versioning

The current repository structure uses:

- `config_version = 3`
- `profile_version = 1`

`Config.config_version` tracks Sheet and automation compatibility. The private profile separately uses `profile_version` in YAML front matter.

## OpenAI references

- Scheduled Tasks: https://help.openai.com/en/articles/10291617
- Connecting and managing app accounts: https://help.openai.com/en/articles/20001494-connecting-and-managing-app-accounts-in-chatgpt
- Google Drive app setup: https://help.openai.com/en/articles/10929079
- Google app data controls: https://help.openai.com/en/articles/10408842-google-app-data-controls-faq

## License

Except where otherwise noted, the original prompts, documentation, examples, and templates in this repository are licensed under **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

You may share and adapt the material, including commercially, as long as you provide appropriate attribution, link to the license, and indicate changes.

Suggested attribution:

> Job Mail Collector by Aiden, licensed under CC BY 4.0.

See `LICENSE` and https://creativecommons.org/licenses/by/4.0/ for details.

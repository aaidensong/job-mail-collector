[English](README.md) | [한국어](README.ko.md)

# Job Mail Collector

Job Mail Collector is a reusable ChatGPT Scheduled Task workflow that reads job-alert emails from Gmail, matches postings against a private career profile, writes suitable jobs into a Google Sheet tracker when permitted, and returns application links in ChatGPT.

The public repository contains only workflow logic and templates. Each user's career profile, email data, and application history stay in that user's connected Google account.

![Job Mail Collector workflow](job-mail-collector-flow.png)

> Product behavior, available apps, and Scheduled Task capabilities can change. Last verified against OpenAI documentation: 2026-09-08.

## What the user actually does

The setup is designed so a new user only needs to do four things:

1. Connect Gmail and Google Drive to ChatGPT.
2. Copy `prompts/01-bootstrap.md` from this repository into a new ChatGPT conversation.
3. Talk through the onboarding questions in that conversation.
4. Open the job links that ChatGPT recommends and apply on the employer's site.

Everything else is handled by ChatGPT when the required permissions are available.

## Onboarding is a conversation, not a form

Users do not need to know the internal schema or provide one exact value per question.

A natural answer such as this is valid:

> I am mainly looking for Senior Product Designer roles, but I would also like to see UX/UI or Lead roles when the actual work fits my experience. I have about nine years of B2C product design experience and a lot of design-system and growth work.

ChatGPT should extract every useful piece of information from the answer, organize it internally, and skip later questions that have already been answered.

The onboarding follows these principles:

- ask exactly one question per turn;
- accept free-form answers that contain several pieces of information;
- collect factual career evidence before asking for narrow restrictions;
- mark direct questions as `Required` or `Optional` when useful, while making clear that no rigid answer format is required;
- explain unfamiliar concepts in plain language;
- skip questions already resolved by earlier answers;
- ask follow-up questions only when the missing information could materially change job matching;
- use recommended defaults for technical settings.

See `docs/profile-questionnaire.md` for the complete question design and `docs/user-flow.md` for the user/GPT responsibility map.

## Fit-based matching instead of title-gated matching

Job Mail Collector does not assume that a user must decide every acceptable title or career level in advance.

A target such as `Senior Product Designer` is treated as an anchor, not automatically as a whitelist.

If a `Staff Product Designer`, `Lead Product Designer`, UX/UI role, or another nearby title has responsibilities and scope that are supported by the user's actual experience, it can still be recommended.

Likewise, a superficially similar title can be downgraded when its actual responsibilities are materially unsupported.

The matcher compares:

- actual responsibilities and role scope;
- required experience and ownership;
- leadership and people-management expectations;
- product and domain fit;
- relevant skills, outcomes, and career evidence;
- location and work-model constraints;
- employment, work authorization, sponsorship, and compensation rules when relevant.

The user mainly needs to identify meaningful hard exclusions, such as roles, domains, management responsibilities, or conditions they definitely do not want.

People management is kept separate from leadership. Project leadership, mentoring, design direction, and cross-functional influence do not automatically imply direct people-management experience.

See `docs/matching-rules.md` for the detailed matching model.

## Where to connect Gmail and Google Drive

In ChatGPT, open `Settings > Apps` or `Settings > Plugins`, depending on the interface available to your account. Connect the Google account that receives your job-alert emails and owns the Drive files you want Job Mail Collector to use.

After the apps are connected, the bootstrap prompt is run in a normal ChatGPT conversation. You do not run a local program, terminal command, Python script, or GitHub Action.

## One-time setup

1. Connect Gmail and Google Drive in ChatGPT.
2. Open `prompts/01-bootstrap.md` on GitHub.
3. Copy the entire prompt into a new ChatGPT conversation.
4. Answer naturally while ChatGPT asks one question at a time.
5. ChatGPT collects career evidence and summarizes how it understands your search.
6. Correct or narrow that interpretation only where needed.
7. ChatGPT creates the private career profile and a new Google Sheet Tracker in your connected Google Drive.
8. If you already have application history, you can optionally choose to import it into the new Tracker.
9. ChatGPT creates a recurring Scheduled Task at the time you selected.
10. ChatGPT runs a validation test before setup is considered complete.

You do not need an existing job tracker before using this project.

You do not manually create or move the generated profile or Tracker files after setup. ChatGPT creates and references them in your connected Drive.

### Existing application history is optional

Job Mail Collector does not search your Drive for an old tracker during initial access checks.

If you say that you have past application history you want imported, ChatGPT can locate it after your explicit authorization and copy compatible history into the new `Job_Mail_Collector` Tracker.

If you have no previous tracker, setup continues normally with a new empty Tracker.

## Daily automation

At the scheduled time, ChatGPT should automatically:

1. load the user's private career profile;
2. read the configured job-alert emails from Gmail;
3. expand digest emails into individual jobs;
4. extract company, role, location, compensation, work model, source, and application links when available;
5. remove explicit mismatches and duplicates;
6. compare the remaining jobs with the user's actual career evidence and constraints;
7. keep plausible nearby or stretch titles when the role scope is supported;
8. write suitable new jobs to the Tracker Sheet when Google Drive write access allows it;
9. return the best matches and application links in the Scheduled Task result;
10. scan clear application confirmations and employer or recruiter responses;
11. update Tracker status fields automatically when the evidence is unambiguous;
12. report anything that requires human review.

The default email scan window is the previous local calendar day. The user only needs to configure a different window if they want one.

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

- career background and actual responsibility scope;
- main role direction;
- nearby roles or career levels to consider when actual fit is strong;
- explicit hard exclusions;
- relevant skills and experience;
- preferred and excluded domains;
- location and work-model rules;
- work authorization and sponsorship handling;
- company and keyword preferences;
- measurable evidence used for job matching;
- leadership evidence kept separate from direct people-management experience.

If direct raw Markdown creation or reading is not supported, ChatGPT creates a private Google Doc containing the same Markdown text and stores that document reference in the Sheet configuration.

### 2. Tracker Sheet

Default name:

`Job_Mail_Collector`

Tabs:

- `Config`: operational settings and automation modes
- `Sources`: confirmed Gmail alert sources
- `Tracker`: job candidates and application history
- `Control`: read-completeness verification

A new Tracker is created automatically during setup.

The career profile is the source of truth for matching. The Sheet does not duplicate the user's full career history.

## Core capabilities

- one-question-at-a-time conversational onboarding
- free-form answers instead of rigid field entry
- evidence-first career profiling
- fit-based title and career-level interpretation
- explicit hard-exclusion handling
- private career profile creation
- automatic creation of a new Google Sheet Tracker
- optional import of existing application history
- Gmail job-alert source discovery and confirmation
- recurring Scheduled Task creation
- digest-email expansion
- hard filtering and evidence-based soft matching
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
├── README.ko.md
├── .gitignore
├── job-mail-collector-flow.png
├── job-mail-collector-flow-ko.png
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
- **Conversation, not a form.** Users can answer naturally and include multiple ideas in one response.
- **Evidence before restrictions.** Collect what the user has actually done before asking them to define narrow role boundaries.
- **One question at a time.** Do not overwhelm users with batch questionnaires.
- **Extract once, do not ask twice.** One answer can resolve several internal fields.
- **Fit over title labels.** Main titles and career levels are anchors, not automatic whitelists.
- **Ask for hard exclusions, not exhaustive allowlists.** Keep plausible roles open unless the user explicitly closes them.
- **People management and leadership are separate concepts.** Direct reports are not inferred from project or mentoring leadership.
- **A new Tracker is created automatically.** Existing history is optional and imported only after explicit user choice.
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

This update changes onboarding and matching interpretation without changing the YAML key set or Sheet schema, so the versions remain unchanged.

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

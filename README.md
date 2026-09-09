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
3. Talk with ChatGPT about the kind of work they want and the experience they bring.
4. Open the job links that ChatGPT recommends and apply on the employer's site.

Everything else is handled by ChatGPT when the required permissions are available.

## Onboarding is a conversation, not a form

Users do not need to know the internal schema or provide one exact value for every question.

For example, this is a valid answer:

> I am mainly looking for Senior Product Designer roles, but UX/UI, Growth, or Lead roles are also interesting when the actual work fits my experience. I have about nine years of B2C product design experience and a lot of design-system work.

ChatGPT should extract every useful piece of information from the answer and skip later questions that have already been answered.

The onboarding follows these rules:

- one question per turn;
- free-form answers are welcome;
- examples are shown when a question may be hard to answer;
- examples are guidance only and do not define a required format;
- normal questions are not labeled `Required`;
- only optional questions are marked as optional;
- optional questions explicitly say they can be skipped;
- career evidence is collected before narrow title or level boundaries;
- technical settings use recommended defaults whenever possible.

In Korean, optional questions use:

`선택 질문입니다. 필요하지 않다면 답변하지 않으셔도 됩니다.`

In English:

`Optional question. You can skip this if it is not useful for your search.`

## The profile goes deeper than title and years

A title such as `Senior Product Designer` is not enough to produce useful matching by itself.

For experienced candidates, the onboarding asks a small number of role-specific depth questions to understand evidence such as:

- what the user actually owned;
- the kinds of problems they are especially good at solving;
- measurable or observable impact;
- decision-making and ambiguity;
- cross-functional influence;
- leadership or mentoring;
- systems or process improvement;
- distinctive specialty areas.

For a product designer, ChatGPT may ask questions such as:

- What kinds of product problems are you especially good at solving?
- What did you own end to end?
- What changed because of your work?
- How did you influence PM, engineering, other designers, or team processes?
- What strength or experience would you most want a hiring team to notice?

The answers are stored in the private profile as evidence used for matching.

## Job fit is based on actual scope, not title alone

Job Mail Collector does not assume that the user must enumerate every acceptable title or career level in advance.

A stated target such as `Senior Product Designer` is treated as an anchor, not a whitelist.

A `Staff Product Designer`, `Lead Product Designer`, UX/UI role, or another nearby title can still be recommended when the actual responsibilities and required scope fit the user's evidence.

Likewise, a job with the same title can still be a weak match when the actual work is materially different.

Matching considers:

- actual responsibilities and role scope;
- required experience and ownership;
- decision-making and leadership expectations;
- direct people-management requirements;
- distinctive strengths and specialty evidence;
- measurable or observable outcomes;
- product and domain experience;
- location and work model;
- employment type, work authorization, sponsorship, and compensation rules.

The matcher should explain Strong and Possible matches using real evidence, not just `same title`.

## People management and leadership are different

Leadership can include project leadership, mentoring, design direction, cross-functional influence, or team-process leadership without direct reports.

People management means direct reports and responsibilities such as 1:1s, performance reviews, hiring, and managing team members.

The workflow records these separately so a hands-on Lead role is not automatically treated like a people-manager role.

## Where to connect Gmail and Google Drive

In ChatGPT, open `Settings > Apps` or `Settings > Plugins`, depending on the interface available to your account.

Connect the Google account that receives your job-alert emails and owns the Drive files you want Job Mail Collector to use.

After the apps are connected, run the bootstrap prompt in a normal ChatGPT conversation.

You do not run a local program, terminal command, Python script, or GitHub Action.

## One-time setup

1. Connect Gmail and Google Drive in ChatGPT.
2. Open `prompts/01-bootstrap.md` on GitHub.
3. Copy the entire prompt into a new ChatGPT conversation.
4. Answer naturally as ChatGPT asks one question at a time.
5. ChatGPT collects enough career evidence to understand scope, impact, and differentiators.
6. ChatGPT summarizes how it currently interprets the search and lets the user correct or narrow it.
7. ChatGPT creates the private career profile and a new Google Sheet Tracker in the connected Google Drive.
8. Existing application history can optionally be imported.
9. ChatGPT creates a recurring Scheduled Task at the selected time.
10. ChatGPT runs a validation test before setup is considered complete.

You do not need an existing job tracker before using this project.

### Existing application history is optional

Job Mail Collector does not search Drive for an old tracker during initial access checks.

If the user says they have past application history they want imported, ChatGPT can locate it after explicit authorization and copy compatible history into the new `Job_Mail_Collector` Tracker.

If there is no previous tracker, setup continues normally with a new empty Tracker.

## Daily automation

At the scheduled time, ChatGPT should automatically:

1. load the user's private career profile;
2. read configured job-alert emails from Gmail;
3. expand digest emails into individual jobs;
4. extract company, role, location, compensation, work model, source, and application links when available;
5. remove obvious hard mismatches and duplicates;
6. compare the remaining jobs with the user's actual career evidence and differentiators;
7. write suitable new jobs to the Tracker Sheet when Google Drive write access allows it;
8. return the best matches and application links in the Scheduled Task result;
9. scan clear application confirmations and employer or recruiter responses;
10. update Tracker status fields automatically when the evidence is unambiguous;
11. report anything that requires human review.

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

Some ChatGPT accounts or managed workspaces can require approval before an external data change.

If a scheduled write cannot proceed without approval, the task must not silently fail. It should return the intended changes as paste-ready TSV and clearly say that Tracker write approval was unavailable.

## Personal data architecture

The workflow uses two private user-owned resources.

### 1. Career profile

Preferred name:

`Job_Mail_Collector_Profile.md`

Contains:

- professional summary;
- fit-based search interpretation;
- differentiators and actual scope;
- experience highlights;
- measurable outcomes;
- core skills and strengths;
- specialty areas;
- leadership and people-management evidence;
- location, work-model, authorization, and preference rules.

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

- conversational onboarding with one question per turn
- free-form answers with examples when useful
- optional-only question labeling
- role-specific depth questions for stronger career evidence
- private career profile creation
- fit-based title and career-level interpretation
- automatic creation of a new Google Sheet Tracker
- optional import of existing application history
- Gmail job-alert source discovery and confirmation
- recurring Scheduled Task creation
- digest-email expansion
- hard filtering and evidence-based matching
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

- **Conversation, not form filling.** Users can answer naturally and provide several pieces of information at once.
- **Examples without rigidity.** Examples help users know what to say without forcing a format.
- **Only optional questions are labeled.** Normal questions are not presented as mandatory form fields.
- **Depth before narrow filters.** Matching should understand scope, impact, and differentiators before asking for fine-grained exclusions.
- **Fit before title labels.** Exact title equality is not required.
- **People management and leadership are separate concepts.**
- **A new Tracker is created automatically.**
- **Private profile outside the public prompt.**
- **No fabricated links.**
- **Hard filters before soft matching.**
- **Evidence with every judgment.**
- **No absence claims from partial data.**
- **Automatic writes with a safe fallback.**
- **No invented application state.**

## Versioning

The current repository structure uses:

- `config_version = 3`
- `profile_version = 1`

These onboarding and matching changes do not alter the YAML key set or Sheet schema, so the configuration and profile versions remain unchanged.

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

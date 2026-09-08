# Profile questionnaire

This document defines the onboarding UX used by `prompts/01-bootstrap.md`.

The user does not fill out this file directly. ChatGPT asks the questions inside the setup conversation and stores the answers in the user's private career profile.

## Conversation rules

The onboarding must be usable by someone who does not know recruiting or HR terminology.

ChatGPT must:

1. ask exactly one question per turn;
2. wait for the answer before continuing;
3. mark every question as `Required` or `Optional`;
4. explicitly allow `none` or `skip` for Optional questions;
5. explain unfamiliar concepts in plain language;
6. use examples when useful;
7. avoid unexplained internal terms such as `seniority`, `adjacent titles`, or `management roles`;
8. use recommended defaults for technical settings;
9. skip questions already answered;
10. ask follow-up clarification before moving on when an answer is ambiguous.

Internal field names can remain technical. User-facing questions should not be.

## User-facing role questions

### Main target role
**Required**

Ask:

> What is the main job title you want Job Mail Collector to look for?

Example: `Senior Product Designer`

Maps to `primary_titles`.

### Similar roles
**Optional**

Ask:

> Are there similar job titles you would also consider?

Explain:

> These are not your first-choice titles, but they are close enough that you would still want to see the job.

Example: if the main target is `Senior Product Designer`, the user might also consider `Product Designer`, `Growth Product Designer`, or `Product Design Lead`.

Maps to `adjacent_titles`.

Do not use the phrase `adjacent titles` without explaining it.

### Job level
**Required**

Ask:

> What level of role are you looking for?

Explain if needed:

> This means the career level of the job, such as Junior, Mid-level, Senior, Staff, Lead, Manager, or Director.

If the user is unsure, collect their experience first and later propose a level for confirmation.

Maps to `target_seniority`.

Do not ask `What is your target seniority?` as the user-facing question.

### Always-excluded roles
**Optional**

Ask:

> Are there any job titles or job levels you never want to see in the recommendations?

Example: `Intern`, `Graphic Designer`, `Director`

Maps job titles to `excluded_titles`. Job-level nuance can be stored in interpretation notes.

### Direct people management
**Required**

Ask:

> Are you open to jobs where you directly manage employees?

Explain:

> People management means having direct reports and responsibilities such as 1:1 meetings, performance reviews, hiring, or managing team members. Leading projects, mentoring, or influencing a team without direct reports does not count as people management here.

Offer:

- Yes, include those roles
- No, individual-contributor roles only
- Review each job individually

Maps internally to `management_roles`.

## People management versus leadership

These must not be treated as the same thing.

### People management

Examples:

- direct reports
- 1:1 meetings
- performance reviews
- hiring responsibility
- managing team members' growth or workload

### Leadership without people management

Examples:

- leading a project
- setting design direction
- mentoring
- running cross-functional reviews
- leading team processes
- facilitating alignment
- owning a major initiative

Ask separately:

> Have you led projects, design direction, mentoring, cross-functional work, or team processes even if nobody directly reported to you?

This is Optional but useful matching evidence.

Leadership without direct reports must not be recorded as proof of people-management experience.

## Career evidence

Collect one item per turn.

### Current or recent role
**Required**

Ask for the current or most recent job title.

### Relevant experience length
**Required**

Ask approximately how many years of experience are relevant to the target jobs.

### Product, industry, or business experience
**Required**

Ask what kinds of products, industries, or business areas the user has worked in most.

### Strongest skills or responsibilities
**Required**

Ask which skills and responsibilities should materially strengthen a match.

### Measurable outcomes
**Optional but recommended**

Ask for concrete outcomes or metrics.

Never invent metrics.

### Leadership evidence
**Optional**

Ask about project, mentoring, process, or cross-functional leadership separately from people management.

A complete chronological resume is not required when a shorter factual profile is enough.

## Domain and skill preferences

### Preferred industries or product types
**Optional**

Maps to `preferred_domains`.

### Excluded industries or product types
**Optional**

Maps to `excluded_domains`.

### Hard skill blockers
**Optional**

Ask only for requirements that should make a posting ineligible when essential and missing.

Maps to `hard_skill_blockers`.

## Geography and work model

### Target location
**Required**

Ask where the user wants to find jobs.

### Work arrangement
**Required**

Offer Remote, Hybrid, On-site, or any combination.

### Commute or relocation constraints
**Optional**

Store material nuance in profile interpretation notes.

## Employment conditions

### Employment type
**Required**

Examples: Full-time, contract, part-time, internship.

### Work authorization
**Required**

Ask in plain language.

Example answer:

`Authorized to work in Canada without sponsorship.`

### Sponsorship handling
**Conditional**

Ask only when sponsorship could materially affect matching.

Offer:

- exclude sponsorship-required jobs
- keep them with a warning
- ignore sponsorship when matching

### Minimum compensation
**Optional**

Only use as a filter when the user wants it.

## Other matching preferences

The following are Optional and should only be asked when useful:

- preferred or avoided company types
- hard-exclude keywords
- warning-only keywords
- languages relevant to work
- resume version labels

Do not turn these into a large batch questionnaire.

## Existing application history

Existing history is Optional.

User-facing question:

> Do you already have a spreadsheet or tracker with past job applications that you want imported into the new Job Mail Collector tracker?

Explain that a new Tracker will be created automatically even when the user has no existing history.

Rules:

- do not search Drive for an existing tracker before the user says yes;
- do not treat a similarly named file as the intended tracker;
- create a new `Job_Mail_Collector` Tracker by default;
- when the user chooses import, copy compatible history into the new Tracker after validating it;
- do not require a previous tracker for setup.

## Automation settings

Do not ask seven technical module questions by default.

Ask one plain-language question:

> Would you like to use the recommended automation settings?

Recommended settings:

- automatically add suitable jobs to Tracker
- detect application confirmation emails
- update clear application status changes
- detect employer or recruiter responses
- estimate rejection stage when evidence supports it
- estimate ATS when recognizable
- flag no-response applications after 14 days

If the user wants customization, ask about changes one at a time.

## Gmail sources

Recommended onboarding question:

> Would you like me to find likely job-alert senders in your Gmail automatically?

If yes, search recent Gmail and show a proposed source list for one confirmation question.

Never enable an unconfirmed sender.

## Schedule

Ask schedule questions near the end of onboarding, not at the beginning.

### Daily run time
**Required**

Ask what time the workflow should run.

### Time zone
**Required unless already clear**

Allow a city name instead of requiring an IANA time-zone string.

Normalize internally.

### Email scan window

Default without requiring technical configuration:

`previous local calendar day, 00:00 through 23:59`

Only ask for a custom window when the user wants to change the default.

## Rule severity

Career-related fields should distinguish between:

- `hard`: can exclude a job
- `soft`: changes match quality but does not exclude
- `info`: context only

The user does not need to learn these labels during onboarding.

The Markdown profile represents severity through fields such as `excluded_titles`, `hard_skill_blockers`, `hard_exclude_keywords`, and narrative interpretation notes.

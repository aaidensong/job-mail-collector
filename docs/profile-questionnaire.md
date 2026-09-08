# Profile questionnaire

This document defines the onboarding UX used by `prompts/01-bootstrap.md`.

The user does not fill out this file directly. ChatGPT collects the information inside the setup conversation and stores the result in the user's private career profile.

## Core principle

The onboarding is a conversation, not a form.

Users may answer in free-form natural language. They do not need to provide one exact value for each question.

For example, this is a valid answer:

> I am mainly looking for Senior Product Designer roles, but I would also like to see UX/UI or Lead roles when the work actually fits my experience. I have about nine years of B2C product design experience and a lot of design-system and growth work.

ChatGPT should extract every useful fact from that answer and use it to resolve multiple profile fields when appropriate.

The user should never feel that they must know the internal schema or provide a precise label such as `Senior`, `Staff`, or `Lead` before the system can judge fit.

## Conversation rules

ChatGPT must:

1. ask exactly one question per turn;
2. allow free-form answers, including answers that contain several pieces of information;
3. extract and normalize all useful information from every answer;
4. skip later questions when the answer has already resolved them;
5. mark each user-facing question as `Required` or `Optional` when a direct question is needed;
6. explain that `Required` means the topic must eventually be understood, not that the user must provide a rigid or exact-format value;
7. explicitly allow `none`, `skip`, `not sure`, or the equivalent for Optional questions;
8. explain unfamiliar concepts in plain language;
9. use examples when useful without implying that the user must copy the example format;
10. avoid unexplained internal terms such as `target_seniority`, `adjacent_titles`, or `management_roles`;
11. collect factual career evidence before asking for narrow title, level, or exclusion boundaries;
12. prefer recommended defaults for technical settings;
13. ask follow-up questions only when unresolved information could materially change job-fit decisions;
14. summarize the inferred search direction before finalizing it and let the user correct the interpretation in natural language.

Internal field names can remain technical. User-facing conversation should not be.

## Phase 1: understand the person and the search

The first phase should collect broad factual context before asking the user to define narrow constraints.

### Opening guidance

Before the first career question, tell the user something equivalent to:

> Answer naturally. You do not need to use a specific format or give only one value. You can mention several roles, preferences, or pieces of experience in one answer. I will organize the useful information and skip questions you have already answered.

### Search direction

**Required**

Ask an open-ended question such as:

> What kind of work are you looking for? Describe it in your own words.

Add a non-restrictive example:

> For example: `I am mainly looking for Senior Product Designer roles, but UX/UI or Lead roles are also interesting when the actual work fits my experience.`

From this answer, ChatGPT may populate or partially populate:

- `primary_titles`
- `adjacent_titles`
- `target_seniority`
- `excluded_titles`
- role interpretation notes
- preferred domains
- strong skills

Do not immediately ask the user to enumerate every acceptable title or career level.

### Current or recent role

**Required if not already known from the conversation**

Ask for the current or most recent role in plain language.

Use it as evidence, not automatically as the user's target.

### Relevant experience

**Required if unresolved**

Ask approximately how much relevant experience the user has.

A rough answer is sufficient.

### Products, industries, and responsibilities

**Required if unresolved**

Ask what kinds of products, industries, business areas, and actual responsibilities the user has worked with most.

The user may answer all of these together.

Do not split them into separate questions unless necessary.

### Strongest skills and evidence

**Required if unresolved**

Ask what the user is strongest at or what they want the matcher to value most.

Allow skills, responsibilities, projects, and outcomes in one natural-language answer.

### Measurable outcomes

**Optional but recommended**

Ask only if concrete evidence is still missing and would improve matching.

Never invent metrics or outcomes.

### Leadership and people management

Leadership and people management are different concepts.

Leadership can include:

- leading projects;
- setting direction;
- mentoring;
- running cross-functional reviews;
- facilitating alignment;
- leading team processes;
- owning major initiatives.

People management means direct reports and responsibilities such as:

- 1:1 meetings;
- performance reviews;
- hiring;
- managing team members' growth or workload.

If the user's answers already make this distinction clear, do not ask again.

If unresolved and likely to affect recommendations, ask one plain-language question at a time.

## Phase 2: infer the search interpretation

After enough factual evidence has been collected, ChatGPT should infer the user's likely search scope.

Do not ask the user to manually classify every role into exact buckets.

Internally, think in three concepts:

### Core target

The user's main role family or direction.

Usually maps to `primary_titles`.

### Consider if fit

Nearby titles, broader role labels, or different career levels that may still be worth applying to when the actual responsibilities and required scope fit the user's evidence.

Usually represented through `adjacent_titles`, `target_seniority`, and natural-language interpretation notes.

Do not require the user to list every possible title.

### Hard exclude

Roles, levels, domains, or conditions the user explicitly does not want.

Usually represented by `excluded_titles`, management rules, excluded domains, hard blockers, or interpretation notes.

## Seniority and title handling

Do not ask questions such as:

> Besides Senior, do you also want Staff or Lead?

unless the distinction is genuinely necessary to resolve a material ambiguity.

The default behavior should be fit-based:

- use the stated title or level as an anchor;
- evaluate nearby or broader titles when the actual scope fits the user's evidence;
- do not exclude a role merely because its title label was not explicitly named during onboarding;
- use explicit exclusions to close categories the user does not want.

Examples:

- A Senior Product Designer target can still include Staff Product Designer when the responsibilities appear supported.
- A Lead Product Designer can be considered when it is an individual-contributor or project-lead role supported by the user's evidence.
- A Design Manager can be excluded if the user explicitly does not want people management.

`target_seniority` should be treated as an anchor, not a whitelist, unless the user's profile explicitly says otherwise.

## Search interpretation checkpoint

Once enough evidence exists, show a concise interpretation and ask one confirmation question.

Example:

> Here is how I currently understand your search:
> - Main direction: Senior Product Designer
> - Also consider when the work fits: Product Designer, UX/UI, Growth, Staff or Lead-level product design
> - Strong evidence: B2C product design, funnel optimization, research, design systems
> - Leadership: project and process leadership
> - Hard exclusions: none confirmed yet
>
> Does anything here need to be corrected or narrowed?

The user can answer freely, for example:

> Lead is fine, but I do not want Manager roles yet. UX/UI is fine only for digital products.

ChatGPT should update all affected fields from that one answer.

## Phase 3: ask only meaningful constraints

After the search interpretation is understood, collect only constraints that materially affect recommendations.

### Hard role or level exclusions

**Optional**

Prefer asking what the user definitely does not want rather than asking them to enumerate everything they would accept.

Example:

> Is there any type of role or level you definitely do not want recommended? If not, you can say `none`.

### Domain preferences and exclusions

Ask only when unresolved or materially useful.

Preferred domains are usually soft preferences.

Explicitly excluded domains can be hard rules.

### Hard skill blockers

**Optional**

Ask only when there is a missing skill or requirement that should make a posting ineligible rather than merely lower its fit.

### Geography and work model

Target location is Required.

Work-model constraints are Required when they materially affect eligibility.

Allow answers such as:

> Toronto mainly, but Canada-remote roles are also fine. Hybrid is okay, but I would rather not be in the office five days a week.

Extract all relevant constraints from one answer.

### Employment conditions

Employment type and work authorization are Required when relevant to eligibility.

Sponsorship handling should only be asked when it could materially change results.

Minimum compensation is Optional.

### Company, keyword, language, and resume preferences

These are Optional.

Ask only when useful and do not turn them into a fixed checklist.

## Existing application history

Existing history is Optional.

Ask:

> Do you already have a spreadsheet or tracker with past job applications that you want imported into the new Job Mail Collector tracker?

Explain that a new Tracker will be created automatically even when the user has no existing history.

Rules:

- do not search Drive for an existing tracker before the user says yes;
- do not treat a similarly named file as the intended tracker;
- create a new `Job_Mail_Collector` Tracker by default;
- when the user chooses import, copy compatible history into the new Tracker after validating it;
- do not require a previous tracker for setup.

## Automation settings

Do not ask several technical module questions by default.

Ask one plain-language question:

> Would you like to use the recommended automation settings?

Recommended settings:

- automatically add suitable jobs to Tracker;
- detect application confirmation emails;
- update clear application status changes;
- detect employer or recruiter responses;
- estimate rejection stage when evidence supports it;
- estimate ATS when recognizable;
- flag no-response applications after 14 days.

If the user wants customization, ask only about the settings they want to change, one at a time.

## Gmail sources

Recommended onboarding question:

> Would you like me to find likely job-alert senders in your Gmail automatically?

If yes, search recent Gmail and show a proposed source list for one confirmation question.

Never enable an unconfirmed sender.

## Schedule

Ask schedule questions near the end of onboarding.

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

Career-related fields should distinguish internally between:

- `hard`: can exclude a job;
- `soft`: changes match quality but does not exclude;
- `info`: context only.

The user does not need to learn these labels during onboarding.

The Markdown profile represents severity through fields such as `excluded_titles`, `hard_skill_blockers`, `hard_exclude_keywords`, management rules, and natural-language interpretation notes.

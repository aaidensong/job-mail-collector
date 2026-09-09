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
5. never label normal questions as `Required`, `필수`, or an equivalent;
6. mark only optional questions;
7. add a sentence to every optional question that clearly says it can be left unanswered if it is not useful;
8. use this exact Korean wording when the conversation is in Korean: `선택 질문입니다. 필요하지 않다면 답변하지 않으셔도 됩니다.`;
9. use this English wording when the conversation is in English: `Optional question. You can skip this if it is not useful for your search.`;
10. explain unfamiliar concepts in plain language;
11. include natural-language answer examples when a question could otherwise be hard to answer;
12. make clear that examples are illustrative and do not define a required format;
13. avoid unexplained internal terms such as `target_seniority`, `adjacent_titles`, or `management_roles`;
14. collect factual career evidence and differentiators before asking for narrow title, level, or exclusion boundaries;
15. prefer recommended defaults for technical settings;
16. ask follow-up questions only when unresolved information could materially change job-fit decisions;
17. summarize the inferred search direction before finalizing it and let the user correct the interpretation in natural language;
18. adapt deeper questions and examples to the user's role family and experience level.

## Opening guidance

Before the first career question, tell the user something equivalent to:

> Answer naturally. You do not need to use a specific format or give only one value. You can mention several roles, preferences, or pieces of experience in one answer. I will organize the useful information and skip questions you have already answered. I will also show example answers when they may help, but you do not need to follow the example format.

This is important because users may otherwise feel that they need to provide exact field values.

## Phase 1: understand the person and the search

The first phase should collect broad factual context and high-signal career evidence before asking the user to define narrow constraints.

### Search direction

Ask an open-ended question such as:

> What kind of work are you looking for? Describe it in your own words.

Example answer:

> I am mainly looking for Senior Product Designer roles, but UX/UI, Growth, or Lead roles are also interesting when the actual work fits my experience.

From this answer, ChatGPT may populate or partially populate:

- `primary_titles`
- `adjacent_titles`
- `target_seniority`
- `excluded_titles`
- role interpretation notes
- preferred domains
- strong skills

Do not immediately ask the user to enumerate every acceptable title or career level.

### Recent role, product, and ownership

If this evidence is not already clear, ask:

> Tell me about your current or most recent role and what you actually owned. You can include your title, the product or problem area, and how far your responsibility extended.

Adapt the example to the user's role family.

Product-design example:

> I was a Senior Product Designer on a recruiting product. I owned the job-application funnel from problem definition and research through interaction design, validation, and launch, working directly with PM and engineering.

The purpose is to understand actual scope, not only the title.

### Relevant experience

Ask only when unresolved:

> About how much experience do you have that is relevant to the kind of work you want next?

Example answer:

> About nine years overall, with the last five focused on B2C product design and recruiting products.

A rough answer is sufficient.

## Role-specific depth interview

A title and years of experience are not enough for reliable matching, especially for experienced candidates.

After the role family and rough level are known, ChatGPT should ask a small number of high-signal depth questions.

Do not mechanically ask every question below. Select only what is missing.

For experienced or senior users, collect evidence across at least three relevant dimensions when possible:

- ownership and scope;
- recurring problem types;
- measurable or observable impact;
- decision-making and ambiguity;
- cross-functional influence;
- leadership or mentoring;
- systems or process improvement;
- distinctive specialty or expertise.

If the user identifies as Senior, Staff, Lead, Manager, Director, Principal, or has substantial experience, normally ask at least two depth questions unless their earlier answers already provide equivalent evidence.

### Problem-solving differentiator

Possible question:

> What kinds of problems are you especially good at solving, or what kinds of problems did teammates tend to rely on you for?

Product-design example:

> I was often brought into ambiguous funnel problems where we knew users were dropping off but did not know why. I combined behavioral data, user research, and product design to find the breakpoint and redesign the flow.

### Impact

Possible question:

> What changed because of your work? This can be a metric, a launch, a better process, reduced risk, faster delivery, better quality, or another concrete outcome.

Example answer:

> I removed an unnecessary review step in an application flow and increased view-to-application conversion by 4 percentage points without a quality drop.

Never invent metrics.

### Influence and leadership

Possible question:

> How have you influenced work beyond your own individual tasks?

Product-design example:

> I ran cross-functional product reviews, mentored a junior designer, helped establish design-system foundations, and stepped in to lead team rituals when needed.

This is leadership evidence. It is not automatically people-management evidence.

### Distinctive strength

Possible question:

> If a hiring team compared you with other people at a similar level, what experience or strength would you most want them to notice?

Product-design example:

> I am strongest at finding funnel breakpoints and turning behavioral data and user research into product changes, especially in B2C marketplace products.

The user does not need to answer in polished resume language.

### Representative work

Possible question:

> What is one project or piece of work that best represents the kind of work you want to be hired for next?

Product-design example:

> A recommendation redesign where I combined funnel data, usability findings, and product strategy, then worked with PM and engineering through launch.

Ask this only when the previous answers still do not show enough specialty or scope.

## Leadership versus people management

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

If the distinction could materially affect recommendations and remains unclear, ask one plain-language question.

Example:

> Have you directly managed employees, or has your leadership mainly been through projects, mentoring, and cross-functional work?

Do not label this as Required.

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
> - Distinctive strengths: B2C funnel optimization, research-informed product design, design systems
> - Scope evidence: end-to-end ownership from problem definition through launch
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

This is optional.

Use the optional marker sentence before the question.

Prefer asking what the user definitely does not want rather than asking them to enumerate everything they would accept.

Example question:

> Is there any type of role or level you definitely do not want recommended?

Example answer:

> I do not want internships or pure people-manager roles, but I am open to Staff or Lead if the work fits.

### Domain preferences and exclusions

Ask only when unresolved or materially useful.

When the question is about preference rather than eligibility, mark it as optional and include the skip sentence.

Preferred domains are usually soft preferences.

Explicitly excluded domains can be hard rules.

### Hard skill blockers

This is optional.

Use the optional marker sentence.

Ask only when there is a missing skill or requirement that should make a posting ineligible rather than merely lower its fit.

### Geography and work model

Ask when unresolved because these conditions affect actual eligibility.

Allow answers such as:

> Toronto mainly, but Canada-remote roles are also fine. Hybrid is okay, but I would rather not be in the office five days a week.

Extract all relevant constraints from one answer.

### Employment conditions

Ask employment type and work authorization when unresolved and relevant to eligibility.

Example answers:

> Full-time is my first choice, but I am also open to longer-term contracts.

> I can work full-time in Canada without employer sponsorship.

Sponsorship handling should only be asked when it could materially change results.

Minimum compensation is optional and should use the optional marker sentence.

### Company, keyword, language, and resume preferences

These are optional.

Ask only when useful and include the optional marker sentence.

Do not turn them into a fixed checklist.

## Existing application history

Existing history is optional.

Use the optional marker sentence.

Ask:

> Do you already have a spreadsheet or tracker with past job applications that you want imported into the new Job Mail Collector tracker?

Example answer:

> No, I am starting from scratch.

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

Ask what time the workflow should run.

Example answer:

> 8:00 AM.

If the timezone is not already clear, ask for the local timezone or city.

Example answer:

> Toronto.

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

# Profile questionnaire

This document defines the onboarding UX used by `prompts/01-bootstrap.md`.

The user does not fill out this file directly. ChatGPT collects information inside the setup conversation and stores the result in the user's private career profile.

## Core principle

The onboarding is a conversation, not a form.

Users may answer in free-form natural language. They do not need to provide one exact value for each question.

Example valid answer:

> I am mainly looking for Senior Product Designer roles, but I would also like to see UX/UI, Growth, or Lead roles when the work actually fits my experience. I have about nine years of B2C product design experience and a lot of design-system and funnel work.

ChatGPT should extract every useful fact from that answer and use it to resolve multiple profile fields when appropriate.

The user should not feel that they must know the internal schema or decide every acceptable title or level in advance.

## Conversation rules

ChatGPT must:

1. ask exactly one question per turn;
2. allow free-form answers containing several pieces of information;
3. extract and normalize all useful information from every answer;
4. skip later questions when an earlier answer already resolved them;
5. never label normal questions as `Required`, `필수`, or equivalent;
6. mark only optional questions;
7. for Korean optional questions, use `선택 질문입니다. 필요하지 않다면 답변하지 않으셔도 됩니다.`;
8. for English optional questions, use `Optional question. You can skip this if it is not useful for your search.`;
9. explain unfamiliar concepts in plain language;
10. include answer examples when useful, while making clear the example format is not required;
11. avoid unexplained internal terms such as `target_seniority`, `adjacent_titles`, or `management_roles`;
12. collect factual career evidence and differentiators before narrow exclusions;
13. prefer recommended defaults for technical settings;
14. ask follow-up questions only when unresolved information could materially change job-fit decisions;
15. summarize the inferred search direction before finalizing it and let the user correct the interpretation naturally.

## Phase 1: understand the person and the search

### Opening guidance

Before the first career question, say something equivalent to:

> Answer naturally. You do not need to use a specific format or give only one value. You can mention several roles, preferences, or pieces of experience in one answer. I will organize the useful information and skip questions you have already answered. I will show example answers when they help, but you do not need to follow the example format.

### Search direction

Ask an open-ended question such as:

> What kind of work are you looking for? Describe it in your own words.

Example answer:

> I am mainly looking for Senior Product Designer roles, but UX/UI, Growth, or Lead roles are also interesting when the actual work fits my experience.

From this answer, ChatGPT may populate or partially populate target titles, nearby roles, career-level clues, role interpretation notes, preferred domains, strong skills, and exclusions.

Do not immediately ask the user to enumerate every acceptable title or career level.

### Recent role, product, and ownership

If still unclear, ask for the current or most recent role and what the user actually owned.

Product-design example:

> I was a Senior Product Designer on a recruiting product. I owned the application funnel from problem definition and research through interaction design, validation, launch, and measurement.

Capture scope and ownership, not only the title.

### Relevant experience

If unresolved, ask approximately how much relevant experience the user has.

Example:

> About nine years overall, with the last five focused on B2C product design and recruiting products.

A rough answer is sufficient.

## High-signal depth questions

For experienced users, title and years are not enough.

Once the role family and rough experience level are known, ask role-specific depth questions one at a time as needed.

For Senior, Staff, Lead, Manager, Director, Principal, or similarly experienced users, normally collect evidence across at least three relevant dimensions and ask at least two depth questions unless earlier answers already provide equivalent evidence.

Useful dimensions:

- ownership and scope
- recurring problem types
- measurable or observable impact
- decision-making under ambiguity
- cross-functional influence
- leadership or mentoring
- systems or process improvement
- specialty or differentiating expertise

Possible product-design questions:

> What kinds of problems are you especially good at solving, or what kinds of problems did teammates tend to rely on you for?

Example:

> I was often brought into ambiguous funnel problems where we knew users were dropping off but did not know why. I combined behavioral data, research, and product design to find the breakpoint and redesign the flow.

Another useful question:

> What changed because of your work? This can be a metric, launch, reduced risk, faster delivery, better quality, or another concrete outcome.

Another:

> How have you influenced work beyond your own individual tasks?

Another:

> If a hiring team compared you with someone who has a similar title and years of experience, what would you want them to understand about what you do particularly well?

Do not ask all examples mechanically. Adapt them to the user's role family.

## Leadership versus people management

Keep these concepts separate.

Leadership can include:

- leading projects or initiatives
- setting direction
- mentoring
- running cross-functional reviews
- facilitating alignment
- leading team processes
- owning major initiatives

People management means direct reports and responsibilities such as:

- 1:1 meetings
- performance reviews
- hiring
- managing team members' growth or workload

If the user's answers already make this clear, do not ask again.
If unresolved and likely to affect recommendations, ask one plain-language question at a time.

## Phase 2: infer the search interpretation

After enough factual evidence exists, infer the user's likely search scope instead of making the user configure a whitelist.

Internally, think in three concepts:

### Core target
The main role family or direction.

### Consider if fit
Nearby titles, broader role labels, or different career levels that can still be worth applying to when actual responsibilities and scope fit the user's evidence.

### Hard exclude
Roles, levels, domains, or conditions the user explicitly does not want.

Do not ask questions such as:

> Besides Senior, do you also want Staff or Lead?

unless that distinction is genuinely necessary.

Use title and level as anchors. Evaluate actual job scope before deciding.

## Search interpretation checkpoint

Once enough evidence exists, show a concise interpretation and ask one confirmation question.

Example:

> Here is how I currently understand your search:
> - Main direction: Senior Product Designer
> - Also consider when the work fits: Product Designer, UX/UI, Growth, Staff or Lead-level product design
> - Strong evidence: B2C product design, funnel optimization, research, design systems
> - Differentiator: diagnosing ambiguous funnel problems using behavioral data and research
> - Leadership: project and process leadership
> - Hard exclusions: none confirmed yet
>
> Does anything here need to be corrected or narrowed?

The user can correct several things in one answer. ChatGPT should update all affected fields.

## Phase 3: ask only meaningful constraints

### Hard role or level exclusions

Optional.
Use the optional-question sentence before asking.
Prefer asking what the user definitely does not want rather than asking for every acceptable role.

### Domain preferences and exclusions

Ask only when unresolved or materially useful.
Preferred domains are usually soft preferences. Explicit exclusions can be hard rules.

### Hard skill blockers

Optional.
Ask only when a missing requirement should make a posting ineligible rather than merely lower its fit.

### Geography and work model

Ask when unresolved.
Allow combined answers such as:

> Toronto mainly, but Canada-remote roles are also fine. Hybrid is okay, but I would rather not be in the office five days a week.

### Employment conditions

Ask employment type and work authorization when relevant.
Ask sponsorship handling only when it could materially change results.

### Minimum compensation

Optional.
Ask only if the user wants compensation filtering.

### Company, keyword, or language preferences

Optional.
Ask only when useful. Do not turn these into a fixed checklist.

Do not ask about resume versions. Resume-version tracking is not part of the core profile or Tracker.

## Existing application history

Existing history is optional.
Use the optional-question sentence before asking whether the user wants to import an existing spreadsheet or tracker.

Rules:

- do not search Drive for an existing tracker before the user says yes;
- create a new `Job_Mail_Collector` Tracker by default;
- import compatible history only after validation;
- when importing an older schema, ignore deprecated ATS, ResumeVersion, Channel, and RejectionStage fields unless a piece of useful context belongs in Notes.

## Automation settings

Ask one plain-language question about recommended automation.

Recommended behavior:

- automatically add suitable jobs to Tracker
- detect clear application-confirmation emails
- detect explicit recruiter submission evidence
- update clear application status changes
- detect employer or recruiter responses
- flag no-response applications after 14 days

Do not configure ATS inference or rejection-stage inference.

If the user wants customization, ask only about requested changes one at a time.

## Gmail sources

Ask whether ChatGPT should find likely job-alert senders automatically.

If yes, search recent Gmail and show a proposed source list for confirmation.
Never enable an unconfirmed sender.

A sender address is not itself a message type. The same sender can produce a job alert, application confirmation, or other content. Message classification should use sender + subject + body.

## Schedule and scan continuity

Ask schedule questions near the end.
Allow a city instead of requiring an IANA timezone string.

Do not ask the user to configure a technical email scan window.
The workflow uses `Control.last_successful_scan_date` to recover missed runs automatically.

On the first run, process the previous local calendar day. Later runs process every unprocessed local calendar day through yesterday.

## Rule severity

Internally distinguish:

- hard: can exclude a job
- soft: changes match quality but does not exclude
- info: context only

The user does not need to learn these labels during onboarding.
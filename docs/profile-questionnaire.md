# Profile questionnaire

This document defines the onboarding UX used by `prompts/01-bootstrap.md`.

The user does not fill out this file directly. ChatGPT collects information inside the setup conversation and stores the result in the user's private career profile.

## Core principle

The onboarding is a conversation, not a form.

Users may answer in free-form natural language. They do not need to provide one exact value for each question.

Fictional example:

> I am mainly looking for Senior Data Analyst roles, but Analytics Engineer or BI Lead roles are also interesting when the work fits. I have about six years of experience, with the last three focused on logistics and operations analytics.

ChatGPT should extract every useful fact from that answer and use it to resolve multiple profile fields when appropriate.

Public examples must be generic and fictional. Do not reuse personal facts from the current user, prior conversations, Memory, or a private career profile as examples.

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
15. summarize the inferred search direction before finalizing it and let the user correct the interpretation naturally;
16. show setup progress and an approximate remaining-answer range before each direct onboarding question.

## Progress visibility

The user should never have to wonder how long setup will continue.

Use four stages:

1. **Career direction & evidence**
2. **Search constraints**
3. **Automation & job-alert sources**
4. **Schedule, create, and test**

At the start, show the roadmap once and explain that most setups take about **8-12 user answers** when recommended settings are used. A detailed answer can cover several topics and reduce the remaining count. Custom settings or new ambiguities can add a few questions.

This number is an estimate, not a quota.

Before each direct question, show one compact line such as:

> Setup progress: 1/4 - Career direction & evidence - about 7-10 answers remaining

In Korean:

> 설정 진행: 1/4 - 구직 방향과 경력 파악 - 약 7-10개의 답변이 남았습니다.

Recalculate the range from unresolved topics after each answer. Use a range rather than false precision. If the estimate increases materially, explain why in one short sentence.

Do not count permission dialogs, connection clicks, or approval taps as user answers.
Do not repeat the entire roadmap every turn.
When the final user question is resolved, tell the user that the Q&A portion is complete and that resource creation and testing will continue automatically where permissions allow.

## Stage 1: career direction and evidence

### Opening guidance

Before the first career question, say something equivalent to:

> Answer naturally. You do not need to use a specific format or give only one value. You can mention several roles, preferences, or pieces of experience in one answer. I will organize the useful information and skip questions you have already answered. I will show example answers when they help, but you do not need to follow the example format.

### Search direction

Ask an open-ended question such as:

> What kind of work are you looking for? Describe it in your own words.

Fictional example:

> I am mainly looking for Senior Data Analyst roles, but Analytics Engineer or BI Lead roles are also interesting when the actual work fits my experience.

From this answer, ChatGPT may populate or partially populate target titles, nearby roles, career-level clues, role interpretation notes, preferred domains, strong skills, and exclusions.

Do not immediately ask the user to enumerate every acceptable title or career level.

### Recent role, domain, and ownership

If still unclear, ask for the current or most recent role and what the user actually owned.

Fictional example:

> I was a Senior Data Analyst on a logistics team. I owned delivery-performance analytics from metric definition and data modeling through dashboard rollout and stakeholder adoption.

Capture scope and ownership, not only the title.

### Relevant experience

If unresolved, ask approximately how much relevant experience the user has.

Fictional example:

> About six years overall, with the last three focused on logistics and operations analytics.

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

Possible question:

> What kinds of problems are you especially good at solving, or what kinds of problems did teammates tend to rely on you for?

Fictional example:

> Teams often disagreed on what an operational KPI meant. I was good at tracing inconsistent data sources, defining a shared metric, and turning it into something teams could use for decisions.

Another useful question:

> What changed because of your work? This can be a metric, launch, reduced risk, faster delivery, better quality, or another concrete outcome.

Fictional answer:

> I automated a weekly reporting workflow and reduced preparation time by about 35%.

Another:

> How have you influenced work beyond your own individual tasks?

Fictional answer:

> I led KPI-definition workshops, mentored a junior analyst, and coordinated with operations and engineering without having direct reports.

Another:

> If a hiring team compared you with someone who has a similar title and years of experience, what would you want them to understand about what you do particularly well?

Do not ask all examples mechanically. Adapt them to the user's role family while keeping examples fictional.

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

## Infer the search interpretation

After enough factual evidence exists, infer the user's likely search scope instead of making the user configure a whitelist.

Internally, think in three concepts:

### Core target
The main role family or direction.

### Consider if fit
Nearby titles, broader role labels, or different career levels that can still be worth applying to when actual responsibilities and scope fit the user's evidence.

### Hard exclude
Roles, levels, domains, or conditions the user explicitly does not want.

Do not ask users to enumerate every acceptable title or level.

Fictional examples:

- A user mainly targeting `Senior Data Analyst` can still receive an `Analytics Engineer` role when the actual scope is supported.
- A `BI Lead` role can be considered when it is compatible with the user's demonstrated ownership.
- A `Data Analytics Manager` role can be excluded when the user explicitly does not want direct people management.

Use title and level as anchors. Evaluate actual job scope before deciding.

## Search interpretation checkpoint

Once enough evidence exists, show a concise interpretation and ask one confirmation question.

Fictional example:

> Here is how I currently understand your search:
> - Main direction: Senior Data Analyst
> - Also consider when the work fits: Analytics Engineer, BI Analyst, BI Lead
> - Strong evidence: SQL, metric design, data modeling, operational analytics
> - Differentiator: turning ambiguous operational questions into reliable metrics across inconsistent data sources
> - Leadership: cross-functional analytics leadership
> - Hard exclusions: none confirmed yet
>
> Does anything here need to be corrected or narrowed?

The user can correct several things in one answer. ChatGPT should update all affected fields.

## Stage 2: search constraints

Ask only unresolved conditions that materially affect recommendations.

### Hard role or level exclusions

Optional. Prefer asking what the user definitely does not want rather than asking for every acceptable role.

### Domain preferences and exclusions

Ask only when unresolved or materially useful.
Preferred domains are usually soft preferences. Explicit exclusions can be hard rules.

### Hard skill blockers

Optional. Ask only when a missing requirement should make a posting ineligible rather than merely lower its fit.

### Geography and work model

Ask when unresolved.

Fictional example:

> Austin is my first choice, but US-remote roles are also fine. Hybrid is okay, but I do not want five days a week on-site.

### Employment conditions

Ask employment type and work authorization when relevant.
Ask sponsorship handling only when it could materially change results.

### Minimum compensation

Optional. Ask only if the user wants compensation filtering.

### Company, keyword, or language preferences

Optional. Ask only when useful. Do not turn these into a fixed checklist.

Do not ask about resume versions. Resume-version tracking is not part of the core profile or Tracker.

## Fresh Tracker policy

Bootstrap always creates a **new Job Mail Collector Tracker** using the workflow's own schema.

Do not ask whether the user already has a spreadsheet or tracker.
Do not search Drive for one.
Do not import, adapt, map, merge, or reuse an existing tracker during bootstrap.

If the user mentions prior application history, explain that setup intentionally starts with a fresh compatible Tracker. If they want old history migrated later, recommend doing that separately in another ChatGPT conversation after setup.

Ongoing Gmail reconciliation is different from spreadsheet migration. A later scheduled run may create a missing Applied row when a clear application-confirmation email or recruiter-submission message proves that an application happened.

If a file named `Job_Mail_Collector` already exists, create a new uniquely named Sheet automatically rather than asking to reuse the old one.

## Stage 3: automation settings and Gmail sources

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

Then ask whether ChatGPT should find likely job-alert senders automatically.
If yes, search recent Gmail and show a proposed source list for confirmation.
Never enable an unconfirmed sender.

A sender address is not itself a message type. The same sender can produce a job alert, application confirmation, or other content. Message classification should use sender + subject + body.

## Stage 4: schedule, create, and test

Ask schedule questions near the end.
Allow a city instead of requiring an IANA timezone string.

Do not ask the user to configure a technical email scan window.
The workflow uses `Control.last_successful_scan_date` to recover missed runs automatically.

Once the final schedule/timezone question is resolved, say that the Q&A portion is complete. Then create the private profile, create a brand-new Tracker, create the Scheduled Task, and run the setup test.

## Rule severity

Internally distinguish:

- hard: can exclude a job
- soft: changes match quality but does not exclude
- info: context only

The user does not need to learn these labels during onboarding.
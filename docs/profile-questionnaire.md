# Profile questionnaire

This document defines the onboarding UX used by `prompts/01-bootstrap.md`.

The user does not fill out this file directly. ChatGPT collects information inside the setup conversation and stores the result in the user's private career profile.

## Core principle

The onboarding is a conversation, not a form.

Users may answer in free-form natural language and may include several useful facts in one response. ChatGPT should extract all useful information, resolve multiple profile fields when appropriate, and skip later questions that are already answered.

The initial setup is not the user's only chance to provide career information. The profile can be improved later, so onboarding should collect enough evidence for useful matching without creating pressure to document an entire career perfectly in one sitting.

If a user is unsure, cannot remember a detail, or wants to add it later, continue once enough information exists for useful matching unless the missing fact is genuinely required for eligibility or core operation.

Public examples must be generic and fictional. Do not reuse personal facts from the current user, prior conversations, Memory, or the private career profile as examples.

## User-facing turn format

Keep every onboarding turn compact.

Do not show a setup roadmap, stage number, stage name, `Setup progress`, `설정 진행`, or an initial estimate of the total number of answers.

For a normal career or preference question, use this order:

1. at most one short context sentence when needed;
2. the question in **bold**;
3. one concise fictional answer example directly below the question;
4. at the very bottom, an approximate remaining-question count.

Korean pattern:

> **최근 또는 현재 역할에서 실제로 어떤 일을 맡았는지 설명해주세요. 직함, 담당 영역, 책임 범위를 함께 말해주시면 됩니다.**
>
> 예: 물류 회사의 Senior Data Analyst로 배송 성과 지표 정의부터 데이터 모델링, 대시보드 배포까지 맡았습니다.
>
> 남은 질문: 약 7개

English pattern:

> **Tell me about your current or most recent role and what you actually owned. You can include your title, domain, and scope.**
>
> Example: I was a Senior Data Analyst on a logistics team and owned delivery-performance analytics from metric definition through dashboard rollout.
>
> Questions remaining: about 7

The question is the primary visual emphasis. Do not bold the remaining-question line.

Before the first career question, use at most one short reassurance sentence. It should make clear that the user can answer naturally, does not need to provide everything now, and can update the profile later.

Example meaning in Korean:

> 편하게 답해주세요. 여러 내용을 한 번에 적어도 되고, 지금 다 정리할 필요는 없습니다. 설정 후에도 경력이나 구직 조건은 언제든 업데이트할 수 있습니다.

## Remaining-question count

The user should know roughly how much Q&A remains without being shown the internal setup process.

Rules:

- Korean: `남은 질문: 약 N개`
- English: `Questions remaining: about N`
- use one approximate integer, not a range;
- recalculate after every answer based on unresolved topics;
- one detailed answer may reduce the number by several questions;
- permission dialogs, connection clicks, approval taps, resource creation, and tests do not count;
- do not imply that a changed estimate means the user answered incorrectly;
- after the final answer, say briefly that the Q&A is complete and continue with setup actions.

## Question examples

Every question about career direction, experience, strengths, constraints, or preferences must show one concise example immediately below the question.

Examples are illustrative only. The user does not need to copy the format.

Prefer examples whose domain, product context, location, years, and metrics are different from the user's actual facts. Do not mirror the user's employer, industry, metric, or other personal career details.

Simple operational yes/no questions such as whether to use recommended automation or whether to auto-discover Gmail sources do not require an example unless the choice could be confusing.

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
9. explain unfamiliar concepts only when necessary;
10. collect factual career evidence and differentiators before narrow exclusions;
11. prefer recommended defaults for technical settings;
12. ask follow-up questions only when unresolved information could materially change job-fit decisions;
13. summarize the inferred search direction before finalizing it and let the user correct the interpretation naturally;
14. do not block setup merely because a useful but nonessential career detail is missing;
15. remind the user only when useful that profile information can be improved after setup.

## Understand the person first

### Search direction

Ask an open-ended question such as:

> **What kind of work are you looking for? Describe it in your own words.**
>
> Example: I am mainly looking for Senior Data Analyst roles, but Analytics Engineer or BI Lead roles are also interesting when the actual work fits my experience.

Do not force the user to provide a single exact title or enumerate every acceptable level.

### Recent role, domain, and ownership

If still unclear, ask what the user actually owned, not only their title.

Fictional example:

> I was a Senior Data Analyst on a logistics team. I owned delivery-performance analytics from metric definition and data modeling through dashboard rollout and stakeholder adoption.

### Relevant experience

If unresolved, ask approximately how much relevant experience the user has.

Fictional example:

> About six years overall, with the last three focused on logistics and operations analytics.

A rough answer is sufficient.

## High-signal depth questions

For experienced users, title and years are not enough.

Collect evidence across useful dimensions such as:

- ownership and scope
- recurring problem types
- measurable or observable impact
- decision-making under ambiguity
- cross-functional influence
- leadership or mentoring
- systems or process improvement
- specialty or differentiating expertise

For Senior, Staff, Lead, Manager, Director, Principal, or similarly experienced users, normally ask at least two depth questions unless earlier answers already provide equivalent evidence.

If the user cannot answer one of these now or prefers to add it later, do not treat that as failed setup. Continue once the profile is strong enough for useful matching.

Possible questions and fictional examples:

> **What kinds of problems are you especially good at solving, or what kinds of problems did teammates tend to rely on you for?**
>
> Example: Teams often disagreed on what an operational KPI meant. I was good at tracing inconsistent data sources, defining a shared metric, and turning it into something teams could use for decisions.

> **What changed because of your work?**
>
> Example: I automated a weekly reporting workflow and reduced preparation time by about 35%.

> **How have you influenced work beyond your own individual tasks?**
>
> Example: I led KPI-definition workshops, mentored a junior analyst, and coordinated with operations and engineering without having direct reports.

Do not ask all examples mechanically.

## Leadership versus people management

Keep leadership separate from direct people management.

Leadership can include leading projects, setting direction, mentoring, cross-functional alignment, reviews, or process leadership.

People management means direct reports and responsibilities such as 1:1s, performance reviews, hiring, or managing team members' growth or workload.

If the user's answers already make this clear, do not ask again.

## Infer the search interpretation

After enough factual evidence exists, infer the user's search scope instead of making the user configure a whitelist.

Internally use:

- **Core target**: the main role family or direction
- **Consider if fit**: nearby titles or broader levels worth evaluating when actual scope fits
- **Hard exclude**: roles, levels, domains, or conditions the user explicitly does not want

Title and level are anchors, not automatic whitelists.

Before moving to practical constraints, show one concise interpretation summary and ask a single bold confirmation question with a fictional example answer.

## Ask only meaningful constraints

Ask only unresolved conditions that materially affect recommendations.

These can include:

- explicit role or level exclusions
- domain preferences or exclusions
- hard skill blockers
- location and work model
- employment type
- work authorization and sponsorship
- minimum compensation
- company, keyword, or language preferences

Each career or preference question should include a concise fictional answer example.

Do not ask about resume versions.

## Fresh Tracker policy

Bootstrap always creates a new Job Mail Collector Tracker using the workflow's own schema.

Do not ask whether the user already has a spreadsheet or tracker. Do not search Drive for one. Do not import, adapt, map, merge, or reuse an existing tracker during bootstrap.

If the user mentions prior application history, explain briefly that setup intentionally starts with a fresh compatible Tracker. Historical migration can be handled separately in another ChatGPT conversation after setup.

If a file named `Job_Mail_Collector` already exists, create a uniquely named new Sheet automatically rather than asking to reuse the old one.

## Automation, sources, and schedule

Use recommended defaults whenever possible and keep user-facing explanations short.

For automation settings, ask one plain-language yes/no question and explain the recommended behavior in one short sentence.

For Gmail sources, ask whether ChatGPT should find likely job-alert senders automatically. If yes, show the proposed source list and ask one confirmation question.

Ask exactly one additional plain-language operational question about Web Discovery:

> **Would you like Job Mail Collector to also search the public web for additional jobs that may not appear in your email alerts?**

Korean:

> **메일 알림에 없는 공고도 Job Mail Collector가 웹에서 추가로 찾아보게 할까요?**

Recommended default: enabled.

Do not ask the user to choose ATS sites, search queries, query counts, weekdays, or search frequency.

For schedule and timezone, ask only what is unresolved. Because schedule and timezone are preferences, include concise fictional examples.

Do not ask the user to configure a technical email scan window. The workflow uses `Control.last_successful_scan_date` to recover missed runs automatically.

## Ongoing profile updates after setup

The private career profile is designed to evolve.

After setup, the user should be able to continue talking naturally in the same conversation. They should not need to know whether a statement belongs in the profile or say `update my profile`.

When the user mentions stable information that is likely to change future matching, ChatGPT should proactively offer to update the profile.

Examples of profile-worthy changes:

- a new role, project, responsibility, or measurable outcome
- a newly clarified strength or specialty
- a change in target roles or career level
- a new hard exclusion
- a change in location, work model, employment type, work authorization, sponsorship, compensation, or people-management preference

Use a short confirmation question such as:

> Would you like me to update your Job Mail Collector profile with this?

In Korean:

> 이 내용을 Job Mail Collector 프로필에 업데이트할까요?

Do not write the profile until the user confirms. If the user explicitly asks for a profile change, that explicit request already counts as confirmation.

Do not propose updates for every casual comment. Only propose them for stable information that is likely to improve or materially change future matching.

After confirmation, update the existing private profile directly. Normal career-profile changes should not require recreating the recurring Scheduled Task because the daily task reloads `Config.profile_reference` on every run.

If the user instead changes operational behavior such as run time, automation modules, Web Discovery enablement, no-response threshold, or Gmail sources, explain what would change and ask for confirmation. After confirmation, update the existing Config, Sources, or Scheduled Task directly when supported instead of asking the user to copy and paste a newly generated scheduled prompt.

## Rule severity

Internally distinguish:

- hard: can exclude a job
- soft: changes match quality but does not exclude
- info: context only

The user does not need to learn these labels during onboarding.
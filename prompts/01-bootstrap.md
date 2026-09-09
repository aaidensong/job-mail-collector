# Bootstrap prompt

Run this once in a normal ChatGPT conversation after connecting Gmail and Google Drive in ChatGPT.

```text
You are setting up Job Mail Collector for me inside ChatGPT.

This is an interactive setup for non-technical users.

The onboarding is a conversation, not a form.

Do not tell me to edit configuration files, move generated files, run terminal commands, manually create a Google Sheet, or manually create a Scheduled Task unless a required capability is unavailable in my account.

Your responsibilities in this setup conversation are to:
1. confirm that Gmail and Google Drive are available to this conversation;
2. understand my job search, career evidence, and differentiators through a natural conversation;
3. create my private career profile in my connected Google Drive;
4. create a new Job Mail Collector Google Sheet tracker in my connected Google Drive;
5. optionally import existing application history only if I explicitly say I want it imported;
6. discover or collect the Gmail job-alert sources I want monitored;
7. configure automatic Tracker writing when supported;
8. create a recurring ChatGPT Scheduled Task at the time I choose;
9. run a test before declaring setup complete.

Do not use ChatGPT Memory, custom instructions, old chats, Project files, uploaded files, or assumptions about me as substitutes for the answers collected in this setup.

Ask onboarding questions in the language I am using when it is clear. Otherwise use English.

[0. APP ACCESS CHECK]

Before asking career questions, verify that you can use the user's connected Gmail and Google Drive in this conversation.

If Gmail is unavailable, tell the user exactly that Gmail must be connected in ChatGPT Settings > Apps or Settings > Plugins, depending on the interface available to the account.

If Google Drive is unavailable, give the same instruction for Google Drive.

Do not continue to final setup until both resources can be accessed.

Do not search Google Drive for an existing job tracker during this access check.

Existing application history is discussed later and only after the user explicitly says they want to import it.

[1. CONVERSATION RULES]

These rules are mandatory.

1. Ask exactly ONE onboarding question per assistant turn.
2. Never display a large batch questionnaire or ask the user to answer several numbered questions at once.
3. Wait for the user's answer before moving to the next question.
4. The user may answer in free-form natural language.
5. The user does NOT need to provide one exact value per question.
6. Extract and normalize ALL useful information from every answer.
7. If one answer resolves several profile fields, fill all of them internally and skip the corresponding later questions.
8. Do not ask again for information that has already been clearly provided.
9. If an answer is ambiguous and the ambiguity could materially change matching, clarify that one point before continuing.
10. Do NOT label normal questions as `Required`, `필수`, or an equivalent.
11. Only optional questions receive an optional marker.
12. For every optional question, include a short sentence that clearly says the user may leave it unanswered if it is not useful.
13. Use this exact wording when the conversation is in Korean:
    `선택 질문입니다. 필요하지 않다면 답변하지 않으셔도 됩니다.`
14. Use this wording when the conversation is in English:
    `Optional question. You can skip this if it is not useful for your search.`
15. For other languages, use a natural equivalent with the same meaning.
16. Do not expose internal schema terms such as `target_seniority`, `adjacent_titles`, or `management_roles` as unexplained user-facing terminology.
17. Explain unfamiliar concepts in plain language before asking.
18. Include a natural-language answer example when a question could otherwise feel abstract or difficult to answer.
19. Examples are illustrative only. Never imply that the user must copy the example format or provide the same fields.
20. Collect factual career evidence and differentiators BEFORE asking the user to define narrow title, level, or exclusion boundaries.
21. Prefer recommended defaults for technical settings instead of making the user configure technical details.
22. Ask a follow-up question only when unresolved information could materially change job-fit decisions or setup behavior.
23. Do not ask the user to enumerate every title or career level they would accept.
24. Default to evaluating plausible roles by actual fit unless the user has explicitly excluded them.
25. Do not accept a senior candidate's title alone as sufficient career evidence. Collect enough scope, impact, problem-solving, and leadership evidence to distinguish strong matches from title-only matches.
26. Adapt deeper questions and examples to the user's role family. Product designers, engineers, marketers, researchers, salespeople, and other roles should not receive identical examples.

Before the first career question, tell the user something equivalent to:

"Answer naturally. You do not need to use a specific format or give only one value. You can mention several roles, preferences, or pieces of experience in one answer. I will organize the useful information and skip questions you have already answered. I will also show example answers when they may help, but you do not need to follow the example format."

Then ask only the first question.

[2. PHASE 1 - UNDERSTAND THE USER FIRST]

The purpose of this phase is to collect broad factual evidence and meaningful differentiators before asking the user to configure narrow matching rules.

Do not show this whole phase list to the user.

A. SEARCH DIRECTION

Ask unless already clearly provided:

"What kind of work are you looking for? Describe it in your own words."

Example answer:
"I am mainly looking for Senior Product Designer roles, but UX/UI, Growth, or Lead roles are also interesting when the actual work fits my experience."

From the answer, extract anything useful, including:
- likely primary_titles
- possible adjacent_titles
- career-level clues
- preferred domains or role directions
- explicit exclusions
- management preferences
- strong skills or specialty areas

Do not force the user to provide a single exact title.

If the user says something like:
"Senior Product Designer를 찾고 있고 UX/UI 쪽도 같이 보면 좋겠다"
understand both the main target and the broader acceptable direction from that answer.

B. RECENT ROLE, PRODUCT, AND OWNERSHIP

Ask only if this evidence is still unclear:

"Tell me about your current or most recent role and what you actually owned. You can include your title, the product or problem area, and how far your responsibility extended."

Adapt the example to the user's role family.

Product-design example:
"I was a Senior Product Designer on a recruiting product. I owned the job-application funnel from problem definition and research through interaction design, validation, and launch, working directly with PM and engineering."

The point is not to collect a job title only. Capture:
- actual scope
- ownership
- product or business context
- cross-functional responsibility
- decision-making level

C. RELEVANT EXPERIENCE

Ask only if unresolved:

"About how much experience do you have that is relevant to the kind of work you want next?"

Example answer:
"About nine years overall, with the last five focused on B2C product design and recruiting products."

A rough answer is enough.

D. HIGH-SIGNAL DEPTH QUESTIONS

Once the user's role family and rough experience level are known, ask role-specific depth questions one at a time.

Do not use all of these mechanically. Select only the questions needed to build a useful profile.

For an experienced or senior candidate, collect enough evidence to understand at least three of these dimensions when they are relevant:
- ownership and scope
- recurring problem types
- measurable or observable impact
- decision-making and ambiguity
- cross-functional influence
- leadership or mentoring
- systems or process improvement
- specialty or differentiating expertise

If the user identifies as Senior, Staff, Lead, Manager, Director, Principal, or has substantial experience, normally ask at least TWO depth questions unless their earlier answers already provide equivalent evidence.

Possible depth question 1 - PROBLEM-SOLVING DIFFERENTIATOR

"What kinds of problems are you especially good at solving, or what kinds of problems did teammates tend to rely on you for?"

Adapt the example to the role.

Product-design example:
"I was often brought into ambiguous funnel problems where we knew users were dropping off but did not know why. I combined behavioral data, user research, and product design to find the breakpoint and redesign the flow."

Possible depth question 2 - IMPACT

"What changed because of your work? This can be a metric, a launch, a better process, reduced risk, faster delivery, better quality, or another concrete outcome."

Example answer:
"I removed an unnecessary review step in an application flow and increased view-to-application conversion by 4 percentage points without a quality drop."

Never invent metrics.

Possible depth question 3 - INFLUENCE AND LEADERSHIP

"How have you influenced work beyond your own individual tasks?"

Product-design example:
"I ran cross-functional product reviews, mentored a junior designer, helped establish design-system foundations, and stepped in to lead team rituals when needed."

This is leadership evidence. Do not treat it as people-management evidence unless the user also had direct reports and management responsibility.

Possible depth question 4 - DISTINCTIVE STRENGTH

"If a hiring team compared you with other people at a similar level, what experience or strength would you most want them to notice?"

Product-design example:
"I am strongest at finding funnel breakpoints and turning behavioral data and user research into product changes, especially in B2C marketplace products."

Do not require the user to describe themselves with polished resume language. Natural answers are acceptable.

Possible depth question 5 - PROUD OR REPRESENTATIVE WORK

"What is one project or piece of work that best represents the kind of work you want to be hired for next?"

Product-design example:
"A recommendation redesign where I combined funnel data, usability findings, and product strategy, then worked with PM and engineering through launch."

Ask this only when the previous answers still do not show enough specialty or scope.

E. LEADERSHIP AND PEOPLE MANAGEMENT

Leadership and people management are different.

Leadership can include:
- leading projects or initiatives;
- setting direction;
- mentoring;
- facilitating cross-functional alignment;
- leading reviews or team processes;
- influencing decisions without direct reports.

People management means direct reports and responsibilities such as:
- 1:1 meetings;
- performance reviews;
- hiring;
- managing team members' growth or workload.

If the user's previous answers already make this clear, do not ask again.

If people-management preference or experience could materially change recommendations and remains unclear, ask one plain-language question.

Example:
"Have you directly managed employees, or has your leadership mainly been through projects, mentoring, and cross-functional work?"

Do not label this as Required.

[3. FIT-BASED ROLE INTERPRETATION]

This is a core rule.

Do not judge eligibility from job-title labels alone.

The user's stated main title or career level is an anchor, not automatically a whitelist.

If a posting's actual responsibilities, required experience, ownership, leadership expectations, and scope are supported by the user's career evidence, the role can still be worth recommending even when the title label is different.

Examples:
- A user mainly targeting Senior Product Designer can still receive Staff Product Designer when the scope is supported.
- A Lead Product Designer can be considered when the work is compatible with the user's experience, including when it is an individual-contributor or project-lead role.
- A Manager role can be excluded when the user explicitly does not want direct people management.
- A superficially similar title should be downgraded when the required scope is materially unsupported.

Do NOT ask questions such as:
"Besides Senior, which of Staff or Lead do you want to include?"
unless that distinction is genuinely necessary to resolve a material ambiguity.

Instead, default to keeping plausible roles open and later ask only about meaningful hard exclusions or strong preferences.

Internally, think in three concepts:

CORE TARGET
The user's main role family or direction.
Usually represented by `primary_titles`.

CONSIDER IF FIT
Nearby titles, title variations, or broader career levels that should still be evaluated when the actual job scope fits the user's evidence.
Represent through `adjacent_titles`, `target_seniority`, and Markdown interpretation notes.
Do not require every plausible title to be enumerated.

HARD EXCLUDE
Roles, levels, domains, or conditions the user explicitly does not want.
Represent through `excluded_titles`, management rules, excluded domains, hard blockers, keywords, or interpretation notes.

Treat `target_seniority` as a matching anchor, not a hard whitelist, unless the user explicitly says otherwise.

[4. SEARCH INTERPRETATION CHECKPOINT]

After enough factual career evidence and differentiators have been collected, summarize the current interpretation before asking narrow constraints.

Keep the summary concise.

Example structure:

"Here is how I currently understand your search:
- Main direction: Senior Product Designer
- Also consider when the work fits: Product Designer, UX/UI, Growth, Staff or Lead-level product design
- Distinctive strengths: B2C funnel optimization, research-informed product design, design systems
- Scope evidence: end-to-end ownership from problem definition through launch
- Leadership: project and process leadership
- Hard exclusions: none confirmed yet

Does anything here need to be corrected or narrowed?"

This is ONE confirmation question.

The user may answer naturally, for example:
"Lead is fine, but I do not want Manager roles yet. UX/UI is fine only for digital products."

Extract all affected rules from that answer.

Do not force the user to approve each bullet individually.

[5. PHASE 2 - ASK ONLY MATERIAL CONSTRAINTS]

After the user's career evidence and search direction are understood, ask only unresolved constraints that could materially change recommendations.

Do not show this whole list to the user.

A. EXPLICIT ROLE OR LEVEL EXCLUSIONS

This is optional.

Use the optional marker sentence required by [1. CONVERSATION RULES].

Prefer asking what the user definitely does NOT want instead of asking them to enumerate everything they would accept.

Example question:
"Is there any type of role or level you definitely do not want recommended?"

Example answer:
"I do not want internships or pure people-manager roles, but I am open to Staff or Lead if the work fits."

Do not ask this if exclusions are already clear.

B. DOMAIN PREFERENCES OR EXCLUSIONS

Ask only if unresolved and useful.

When the question is only about preference rather than eligibility, treat it as optional and use the optional marker sentence.

Example answer:
"I prefer B2C products and marketplaces, but I am open to other digital products if the role is strong."

Preferred domains are generally soft preferences unless the user says otherwise.

Explicitly excluded domains can be hard rules.

C. HARD SKILL BLOCKERS

This is optional.

Use the optional marker sentence.

Ask only if the user has a requirement that should make a posting ineligible rather than merely lower its fit.

D. LOCATION AND WORK MODEL

Ask when unresolved because it affects actual eligibility.

Allow free-form combined answers such as:
"Toronto mainly, but Canada-remote roles are also fine. Hybrid is okay, but I would rather not be in the office five days a week."

Extract target_locations, work_models, and material constraints from one answer.

E. EMPLOYMENT TYPE

Ask when unresolved.

Example answer:
"Full-time is my first choice, but I am also open to longer-term contracts."

Allow multiple acceptable types in one answer.

F. WORK AUTHORIZATION

Ask when relevant to eligibility.

Ask in plain language.

Example answer:
"I can work full-time in Canada without employer sponsorship."

G. SPONSORSHIP HANDLING

Ask only when sponsorship could materially change matching and the rule is unresolved.

H. MINIMUM COMPENSATION

This is optional.

Use the optional marker sentence.

Ask only if the user wants compensation filtering.

I. COMPANY, KEYWORD, LANGUAGE, AND RESUME PREFERENCES

These are optional.

Ask only when useful. Use the optional marker sentence for any question in this group.

Do not turn these into a fixed checklist.

[6. EXISTING APPLICATION HISTORY]

This is optional.

Use the optional marker sentence.

Ask:
"Do you already have a spreadsheet or tracker with past job applications that you want imported into the new Job Mail Collector tracker?"

Example answer:
"No, I am starting from scratch."

Explain:
"If not, that is completely fine. I will create a new empty tracker automatically."

Rules:
- Do not search Google Drive for an existing tracker before the user says yes.
- A new `Job_Mail_Collector` tracker is the default for every new setup.
- Existing application history is optional.
- If the user answers no or skips, create a new empty Tracker and continue.
- If the user answers yes, then and only then ask permission to locate the existing tracker or ask the user to identify it.
- Import compatible history into the new Job Mail Collector Tracker after validating the source.
- Do not silently replace the new Tracker with an arbitrary existing spreadsheet.
- Do not infer that a similarly named file is the user's intended tracker.

[7. AUTOMATION SETTINGS]

Ask one plain-language question:
"Would you like to use the recommended automation settings?"

Explain the recommended behavior in plain language:
"Recommended settings automatically add suitable jobs to the tracker, detect application confirmation emails, detect employer or recruiter replies, detect rejections, estimate the ATS when possible, and flag applications with no response after 14 days."

Offer:
- Yes, use recommended settings
- No, customize them

Recommended defaults:
- automatic Candidate writes: enabled
- missing application detection: enabled
- automatic application status updates: enabled
- response detection: enabled
- rejection-stage estimation: enabled
- ATS estimation: enabled
- no-response detection: enabled
- no-response days: 14

If the user chooses custom settings, ask only about the changes they want, one at a time.

Explain only when relevant that external write actions can require approval depending on the user's account or workspace. If unattended writes are blocked, the Scheduled Task will return TSV as a fallback instead of silently failing.

[8. JOB-ALERT SOURCES]

Ask:
"Would you like me to find likely job-alert senders in your Gmail automatically?"

Explain:
"Recommended: I can check recent Gmail messages for sources such as LinkedIn, Indeed, Glassdoor, company career alerts, or recruiting agencies, then show you the proposed senders for confirmation."

Offer:
- Yes, find them automatically
- No, I will provide them myself

If auto-discover is chosen:
- search the last 30 days of connected Gmail for likely job-alert senders;
- show the proposed source list;
- ask one confirmation question about that proposed list;
- never enable a sender the user has not confirmed.

If manual is chosen:
- ask for one sender or domain at a time until the user says they are done.

For digest sources, record that one email can contain multiple jobs.

[9. SCHEDULE]

Ask schedule questions near the end of onboarding.

Question 1:
"What time should Job Mail Collector run each day?"

Example answer:
"8:00 AM."

Question 2 only if timezone is not already clear:
"Which local time zone should that schedule follow? If you tell me your city, I can use the correct time zone."

Example answer:
"Toronto."

Normalize internally to an IANA time zone such as `America/Toronto`.

SCAN WINDOW DEFAULT

Do not make the user configure an email scan window unless they ask to change it.

Default:
previous local calendar day, 00:00 through 23:59 in the selected schedule time zone.

Mention this default in the setup summary.

[10. RESOLVE NOT-SURE ANSWERS]

When the user answers `not sure`:

1. continue collecting surrounding factual information when useful;
2. infer a reasonable interpretation from the evidence;
3. present that interpretation later for confirmation;
4. never invent a preference without giving the user a chance to correct it.

For example, if the user is unsure about career level, do not force a Senior, Staff, or Lead selection immediately. Use their experience and scope as evidence and keep plausible levels open unless the user later excludes them.

[11. CREATE PRIVATE CAREER PROFILE]

After career-related answers are sufficiently complete, create a private profile owned by the user.

Preferred storage:
- create a private Google Drive file named `Job_Mail_Collector_Profile.md` when direct Markdown file creation and later reading are supported.

Compatibility fallback:
- if direct raw Markdown creation or reading is unavailable, create a private Google Doc named `Job_Mail_Collector_Profile`;
- put the exact Markdown text into that Google Doc;
- use that Google Doc as the profile source.

The user should not have to download, upload, or move this profile after setup.

Use this exact YAML key set:

---
profile_version: 1
primary_titles: []
adjacent_titles: []
target_seniority: []
excluded_titles: []
management_roles: review-needed
preferred_domains: []
excluded_domains: []
strong_skills: []
hard_skill_blockers: []
target_locations: []
work_models: []
employment_types: []
work_authorization: ""
sponsorship_rule: warning
minimum_compensation: ""
preferred_company_types: []
excluded_company_types: []
hard_exclude_keywords: []
warning_keywords: []
languages: []
resume_versions: []
---

# Job Mail Collector Career Profile

## Professional summary
Write a concise factual career summary based only on the user's answers.

## Search interpretation
Summarize the main role direction, nearby roles to consider when actual fit is strong, and explicit hard exclusions.
State clearly when career-level labels are anchors rather than whitelists.

## Differentiators and scope
Record the evidence that distinguishes this user from another candidate with a similar title.
Include supported information such as:
- recurring problem types they are especially good at solving;
- end-to-end ownership or decision-making scope;
- distinctive domain or product expertise;
- cross-functional influence;
- systems, process, or organizational impact;
- the kind of work they want to be hired for next.

Do not turn generic self-description into fact without supporting context.

## Experience highlights
Record the roles, products, domains, responsibilities, and scope needed for job matching.

## Measurable outcomes
Record only outcomes or metrics actually provided.

## Core skills and strengths
Record strengths that should materially increase job fit.

## Portfolio or specialty areas
Record specialty areas that should influence matching.

## Leadership experience
Record project leadership, mentoring, cross-functional leadership, process leadership, and direct people-management evidence separately.
Do not treat leadership without direct reports as proof of people management.

## Role preferences and interpretation notes
Capture nuance that does not fit cleanly in the YAML fields.

## Additional context
Add only stable career context that materially improves matching.

Before saving:
- verify the YAML field names exactly match the schema;
- verify the YAML can be interpreted reliably;
- verify no career fact was invented;
- verify hard exclusions and warning-only preferences are distinct;
- verify people management and non-managerial leadership are not conflated;
- verify primary_titles and target_seniority are not accidentally treated as hard whitelists unless the user explicitly requested that behavior;
- verify the Markdown body contains enough evidence to judge fit by actual responsibilities and scope;
- for an experienced or senior user, verify the profile contains meaningful differentiators beyond title and years of experience.

[12. CREATE NEW GOOGLE SHEET TRACKER]

Create a new Google Sheet named `Job_Mail_Collector` unless the user chooses another name.

This is the default behavior for a new setup.

The user should not have to create or move the Sheet manually.

Do not require an existing tracker.

Do not search for or reuse an existing tracker unless the user explicitly asked to import existing application history during onboarding.

If a file named `Job_Mail_Collector` already exists:
- do not overwrite it silently;
- ask one question asking whether to reuse that Job Mail Collector file or create a new uniquely named one.

Create these tabs exactly:
- Config
- Sources
- Tracker
- Control

Config columns:
A Key
B Value
C Notes

Required Config rows:
config_version | 3
profile_reference | exact reference or link to the private profile
profile_storage_format | markdown_file OR google_doc_markdown
schedule_time | selected time
schedule_timezone | selected time zone
scan_window | configured email window
auto_candidate_write | true/false
auto_application_update | true/false
module_missing_application | true/false
module_response_detection | true/false
module_no_response | true/false
no_response_days | configured number
module_rejection_stage | true/false
module_ats | true/false
write_fallback | tsv

Sources columns:
A Source
B SenderPattern
C Enabled
D DigestMode
E LinkRule
F Notes

Tracker columns:
A Status
B Company
C Title
D Location
E Salary
F WorkMode
G Notes
H Link
I ReceivedAt
J AppliedAt
K RespondedAt
L Result
M RejectionStage
N Channel
O ATS
P ResumeVersion
Q Source

Status values:
Candidate
Applied
Closed
Excluded

Control columns:
A Metric
B Value
C Notes

Create these Control rows:
config_version | 3
tracker_data_rows | formula that counts non-empty Tracker company rows below the header
candidate_count | formula for Status=Candidate
applied_count | formula for Status=Applied
closed_count | formula for Status=Closed
excluded_count | formula for Status=Excluded

Do not duplicate the user's full career profile into the Sheet.

[13. OPTIONAL IMPORT OF EXISTING APPLICATION HISTORY]

Run this section only if the user explicitly chose to import existing history.

The new `Job_Mail_Collector` Sheet remains the primary tracker.

Locate or read the existing tracker only with the user's explicit authorization.

Map compatible historical fields into the new Tracker schema.

Do not overwrite newer data.
Do not invent missing values.
Do not import formulas as if they were user data.
Report rows that cannot be mapped safely.

If import fails, keep the new Tracker and continue setup with an empty or partially imported Tracker after reporting the issue.

[14. VERIFY TRACKER WRITE CAPABILITY]

If auto_candidate_write or auto_application_update is enabled, test whether a supported Google Drive action can update the Sheet with the user's current permissions.

Do not add fake job data to Tracker to perform this test.

Use a harmless supported method such as writing and restoring a dedicated Config test value when possible.

Set the operating behavior:
- AUTO_WRITE_AVAILABLE: scheduled runs should write directly to Tracker.
- APPROVAL_OR_WRITE_UNAVAILABLE: scheduled runs should return exact intended changes as TSV and clearly report that the write was not applied.

Do not claim automatic writing works unless the capability was actually verified.

[15. CREATE THE SCHEDULED TASK]

Use the full prompt at:
https://github.com/woosuksong/job-mail-collector/blob/main/prompts/02-daily-job-mail-collector.md

Replace `{{SHEET_REFERENCE}}` with the exact Sheet created during setup.

Create a recurring ChatGPT Scheduled Task at the time and time zone selected by the user.

The task runs inside ChatGPT. The user does not need to run a program, keep a browser tab open, or manually trigger the daily workflow.

If the interface requires the user to approve creation of the task, ask for that approval at the point the product requires it.

Do not embed the user's detailed career profile inside the task prompt. The recurring task reads the profile reference from the Sheet every time.

If you cannot access the linked daily prompt directly, explain this limitation and ask the user to paste `02-daily-job-mail-collector.md` only at that point. Do not ask for it earlier.

[16. TEST RUN]

Before setup is complete, run the workflow once against a small recent window, preferably the previous local calendar day.

Verify:
- private profile can be read;
- required YAML can be parsed;
- Gmail can be searched;
- every enabled source can be searched;
- digest emails are expanded into individual jobs;
- the Sheet can be read;
- Control.tracker_data_rows matches the number of non-empty Tracker rows returned;
- at least one application link can be parsed when matching emails exist;
- automatic Tracker writing is available when enabled, or the TSV fallback is correctly configured;
- the test does not reject plausible roles solely because the title or career-level label differs from the user's main target;
- the profile contains enough evidence to explain why a role is a strong or possible match beyond exact title similarity.

If the test window has no enabled-source email, use the nearest recent date that contains one.

[17. SETUP COMPLETION OUTPUT]

Return:
1. profile name and exact reference;
2. profile storage format;
3. new Sheet name and link/reference;
4. imported history status, if applicable;
5. Scheduled Task name, run time, and timezone;
6. scan-window default or custom setting;
7. enabled Gmail sources;
8. concise fit-based search interpretation without reproducing the full career history;
9. concise differentiators that will strengthen matching;
10. explicit hard exclusions, if any;
11. enabled automation behavior in plain language;
12. Tracker write mode: AUTO_WRITE_AVAILABLE or APPROVAL_OR_WRITE_UNAVAILABLE;
13. test result;
14. any permission, parsing, profile, Gmail, Sheet, or scheduling issue still requiring user action.

Do not declare setup complete if Gmail, the private profile, the new Sheet, or Scheduled Task creation failed.
```
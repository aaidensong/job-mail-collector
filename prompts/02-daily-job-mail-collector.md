# Daily Job Mail Collector prompt template

`{{SHEET_REFERENCE}}` is replaced by the bootstrap flow with the user's own Google Sheet reference.

```text
Run my daily Job Mail Collector using this Google Sheet for operational configuration and tracking:
{{SHEET_REFERENCE}}

Do not rely on ChatGPT Memory, custom instructions, old chats, Project files, uploaded files, or assumptions about me.

My career data is stored in the private profile referenced by Config.profile_reference. Read that profile on every run before evaluating job fit.

[1. LOAD OPERATIONAL CONFIGURATION]

Read Config, Sources, Tracker, and Control.

Require Config.config_version = 4.
Use Config.schedule_timezone for all date and time judgments and output.
Use only Sources rows where Enabled is true.

Required Config keys:
profile_reference
profile_storage_format
schedule_timezone
scan_window
auto_candidate_write
auto_application_update
module_missing_application
module_response_detection
module_no_response
no_response_days
write_fallback

Do not expect ATS, resume-version, channel, or rejection-stage configuration. These are not part of the core workflow.

[2. DETERMINE TARGET PERIOD AND CATCH UP MISSED RUNS]

Use local calendar dates in Config.schedule_timezone.

Set:
- target_end = previous local calendar day
- if Control.last_successful_scan_date is blank, target_start = target_end
- otherwise target_start = day after Control.last_successful_scan_date

Process every local calendar day from target_start through target_end.

If target_start is after target_end, report `No unprocessed calendar day` and skip candidate ingestion for this run. You may still perform non-date-dependent diagnostics when useful.

At the very top of the output, show:
`Scan period: YYYY-MM-DD to YYYY-MM-DD (timezone)`

Do not infer the scan period from the newest Tracker row. A day can be processed successfully even when no candidate was added.

Do not update Control.last_successful_scan_date yet. That happens only after successful core processing.

[3. VERIFY TRACKER READ COMPLETENESS]

Before any Tracker-dependent absence, duplicate, response, or missing-application judgment:
1. Count non-empty Tracker rows using Company as the required field.
2. Compare that count with Control.tracker_data_rows.
3. If equal, tracker_read_status = VERIFIED.
4. If unequal, retry once using the broadest available Sheet read method.
5. If still unequal, tracker_read_status = INCOMPLETE.

When tracker_read_status = INCOMPLETE:
- continue source-health and basic Gmail parsing when useful;
- do not claim a job or application is absent from Tracker;
- do not confirm historical duplicates;
- skip response, missing-application, and no-response reconciliation;
- do not write new Candidate rows because historical duplicate checks are unverified;
- report the row-count mismatch in Diagnostics;
- do not advance Control.last_successful_scan_date.

Partial data must never be used to prove absence.

[4. LOAD AND VALIDATE PRIVATE CAREER PROFILE]

Read the complete document referenced by Config.profile_reference.

Config.profile_storage_format may be:
- markdown_file
- google_doc_markdown

Treat the text as Markdown with YAML front matter.
Require profile_version = 2.

Required YAML keys:
profile_version
primary_titles
adjacent_titles
target_seniority
excluded_titles
management_roles
preferred_domains
excluded_domains
strong_skills
hard_skill_blockers
target_locations
work_models
employment_types
work_authorization
sponsorship_rule
minimum_compensation
preferred_company_types
excluded_company_types
hard_exclude_keywords
warning_keywords
languages

Also read these Markdown sections when present:
- Professional summary
- Search interpretation
- Differentiators and scope
- Experience highlights
- Measurable outcomes
- Core skills and strengths
- Portfolio or specialty areas
- Leadership experience
- Role preferences and interpretation notes
- Additional context

Set profile_read_status:
- VERIFIED
- INVALID
- UNAVAILABLE

If not VERIFIED:
- do not classify jobs as Strong, Possible, or Weak;
- do not apply personal hard filters from the unavailable profile;
- do not infer the user's target from Memory or prior task output;
- do not write candidate rows;
- report PROFILE_INVALID or PROFILE_UNAVAILABLE;
- do not advance Control.last_successful_scan_date.

[5. COLLECT JOB-ALERT MESSAGES]

For every enabled Sources.SenderPattern, search Gmail for messages received during the full target period.

Read messages, not only thread summaries.
Read each message individually even when Gmail grouped several messages into one thread.

If DigestMode is true, open the body and extract every distinct job in the message. Do not assume the subject contains the only job.

For each extracted job, capture when available:
- company
- title
- location
- salary
- work mode
- source
- received timestamp converted to Config.schedule_timezone
- raw job/application URL
- evidence needed for fit judgment

Do not invent missing company names, titles, salaries, locations, or links.

[6. CLASSIFY MESSAGES BEFORE USING THEM]

Do not assume a sender always represents one message type.

Classify each relevant message using sender + subject + body into one of these useful categories when possible:
- job alert
- application confirmation
- recruiter submission evidence
- employer or recruiter response
- marketing/newsletter
- unknown

A sender can produce multiple categories across different messages.

Candidate collection uses only messages that actually contain identifiable open jobs.
Application confirmations and recruiter-submission evidence are retained for reconciliation when the relevant module is enabled.

If classification is uncertain, keep the message as unknown or Human review rather than forcing a category.

[7. NORMALIZE LINKS]

Prefer a stable directly usable job or application URL.

For LinkedIn, when a job ID is explicit, normalize to:
https://www.linkedin.com/jobs/view/{JOB_ID}/

Remove tracking parameters from normalized LinkedIn URLs.

For other sources, preserve a usable job/application URL from the message or linked page.

If the URL is missing, broken, inaccessible, or cannot be associated with the correct job confidently:
- leave Link blank;
- add `link not extracted` to Notes;
- never reconstruct or guess a URL.

If a posting is clearly expired or removed, keep the URL only if useful for identification and exclude the job with reason `posting unavailable`.

[8. DEDUPLICATE WITHIN THE CURRENT RUN]

Primary duplicate key: normalized Company + normalized Title.
Use location as a tie-breaker when the same title clearly represents different openings.

If the same posting appears from multiple sources, keep one candidate and combine the source names in Source, for example `Glassdoor + LinkedIn`.
Prefer the cleanest usable link.

Do not infer a parent company from an unfamiliar subsidiary or brand name.
Preserve the source wording.
If two records may be the same job but company identity is uncertain, mark `suspected duplicate` in Human review instead of merging automatically.

[9. APPLY EXPLICIT HARD FILTERS]

Run only when profile_read_status = VERIFIED.

Use explicit profile rules, including when relevant:
- excluded_titles
- excluded_domains
- disallowed employment types
- hard location/work-model constraints
- sponsorship/work-authorization blockers according to sponsorship_rule
- hard_exclude_keywords
- hard_skill_blockers
- explicit management constraints

A hard exclusion must have a short evidence-based reason.
Missing information is not automatically a hard exclusion unless the profile explicitly says so.

Title and level rules:
- primary_titles identifies the main direction, not an exact-title whitelist;
- adjacent_titles is not exhaustive;
- target_seniority is an anchor, not a whitelist;
- do not reject a role merely because its exact title or level was not listed;
- only explicit exclusions should close that category.

Management handling:
- `unacceptable`: exclude roles that clearly require direct people management;
- `review-needed`: keep them with a warning;
- `acceptable`: evaluate normally.

Do not confuse project leadership, mentoring, design direction, or cross-functional influence with direct people management.

[10. EVALUATE ACTUAL FIT]

Run only when profile_read_status = VERIFIED.

Judge the actual job, not title similarity alone.

Use evidence from both YAML and Markdown, especially Differentiators and scope.
Consider:
- actual responsibilities and role scope
- required experience and ownership level
- decision-making and ambiguity
- leadership expectations
- people-management requirements
- distinctive problem-solving strengths
- demonstrated differentiators and specialty areas
- product/domain fit
- skills and experience fit
- relevant measurable outcomes
- location/work-model fit
- employment/compensation fit when known

A user mainly targeting Senior can still receive Staff or Lead roles when scope is plausible for their experience.
A role can be Strong even when the exact title differs from primary_titles.
A superficially similar title can be Weak or Excluded when the required scope is unsupported.

Stretch roles:
- if much of the required scope is supported, keep as Strong or Possible depending on evidence and mention the stretch when useful;
- if materially unsupported scope is required, lower the fit or exclude using the concrete reason.

Classify:
- Strong match
- Possible match
- Weak match

Do not create false precision with numeric scores unless the private profile explicitly requests one.

For an experienced user, a Strong match should normally include at least one meaningful reason beyond exact title similarity, such as matching problem type, ownership scope, measurable outcome, specialty, domain depth, or leadership evidence.

Weak matches normally stay out of the main shortlist.

[11. COMPARE AGAINST TRACKER]

Run confirmed historical comparison only when tracker_read_status = VERIFIED.

Rules:
- same Company + same Title: historical duplicate, do not add unless evidence shows a materially different requisition;
- same Company + different Title: keep, but add concise prior-company context when useful;
- staffing or recruiting agencies are not automatically the employer. Do not use an agency name by itself to prove a duplicate.

There is no separate Channel field. Use Source for provenance and Notes for agency/recruiter context when it materially helps the user.

If a previously excluded posting was unavailable or expired, a clearly new requisition can be reconsidered when evidence shows it is a new opening.

[12. SINGLE-PASS INBOX RECONCILIATION]

Run when module_missing_application or module_response_detection is enabled AND tracker_read_status = VERIFIED.

Do NOT run a separate Gmail search for every Applied row by default.

Instead:
1. read Gmail messages received in the full target period in one broad pass;
2. build a list of relevant Tracker rows, especially Status=Applied and recent Candidate rows;
3. classify the target-period inbox messages using sender + subject + body;
4. compare potential confirmations, recruiter submissions, and employer/recruiter responses against Company + Title + thread/context evidence;
5. use a targeted follow-up search only when needed to resolve a specific ambiguity.

This inbox pass is separate from the enabled-source candidate-alert searches because employer and recruiter responses may come from completely different senders.

[13. MISSING APPLICATION DETECTION]

Run only if Config.module_missing_application is enabled AND tracker_read_status = VERIFIED.

Use clear application evidence from the single-pass inbox scan.

Strong evidence includes:
- an explicit application-confirmation email naming the company and role;
- an explicit recruiter message stating that the user's application, profile, or resume was submitted or forwarded for a specific company/role.

If Company + Title is not present in Tracker and the evidence is clear, create an Applied row when automatic application updates are enabled and writes are available. Otherwise return the 13-column TSV fallback.

For a general career-page submission with no role title, preserve the source wording and use a non-colliding title such as `Unknown (Career Page)` only when the message truly provides no role title.

Ambiguous language such as a recruiter saying they may submit the user later is not enough. Put it in Human review.

AppliedAt = the evidence message timestamp converted to Config.schedule_timezone when no better confirmed application time is available.
Source = the actual evidence source, for example `LinkedIn`, `Company email`, or `Recruiter email`.

[14. RESPONSE DETECTION]

Run only if Config.module_response_detection is enabled AND tracker_read_status = VERIFIED.

Use the single-pass inbox messages and Tracker rows with Status=Applied.

A response must be received after AppliedAt and have enough company/title/thread context to associate it with the application.
Do not treat generic alerts or unrelated company marketing mail as a response.

When evidence is clear:
- fill RespondedAt if blank;
- if the message explicitly rejects the application, set Status=Closed and Result=Rejected;
- if it explicitly communicates another final result, store that result in Result when unambiguous;
- do not infer rejection stage;
- do not infer or store ATS platform.

Ambiguous or conflicting evidence goes to Human review and does not automatically change the row.

[15. NO-RESPONSE CHECK]

Run only if Config.module_no_response is enabled AND tracker_read_status = VERIFIED.

Find Tracker rows with:
- Status=Applied
- RespondedAt blank
- elapsed local calendar days since AppliedAt >= Config.no_response_days

Return them as no-response candidates.
Do not automatically close them or set Result=No response unless the user explicitly configured that behavior outside the core workflow.

[16. TRACKER WRITES]

Tracker schema is exactly 13 columns:
Status	Company	Title	Location	Salary	WorkMode	Notes	Link	ReceivedAt	AppliedAt	RespondedAt	Result	Source

Do not output or write ATS, ResumeVersion, Channel, or RejectionStage fields.

For new candidates:
- Status = Candidate
- ReceivedAt = original job-alert timestamp in Config.schedule_timezone
- leave AppliedAt, RespondedAt, and Result blank unless supported by reconciliation evidence
- Source = alert source or combined alert sources

For automatic status reconciliation:
- clear application evidence -> Status=Applied and AppliedAt if blank;
- clear employer or recruiter response -> fill RespondedAt if blank;
- explicit rejection -> Status=Closed, Result=Rejected;
- other explicit final outcome -> Status=Closed and store the supported Result;
- ambiguous evidence -> no automatic change.

Preferred mode:
- if automatic writing is enabled and permitted, apply changes directly;
- never overwrite user-entered data with a lower-confidence inference;
- reread affected rows when possible and report whether the change was applied.

Fallback mode:
- if a write cannot proceed because approval is required or the action is unavailable, return every intended insert or update as a fenced 13-column TSV block;
- clearly state that the automatic write was not applied.

[17. ADVANCE THE SUCCESSFUL-SCAN MARKER]

Advance Control.last_successful_scan_date to target_end only when ALL of the following are true:
- profile_read_status = VERIFIED;
- tracker_read_status = VERIFIED;
- Gmail access worked;
- every enabled source search for the target period completed without access/query failure;
- the full target period was processed;
- normal output was produced;
- intended Tracker changes were either applied successfully or returned completely through the explicit TSV fallback.

Zero messages from a source is not itself a failure.
A source query/access failure is a failure.

If any core condition above fails, do not advance last_successful_scan_date. This allows the next scheduled run to catch up automatically.

[18. OUTPUT]

Always produce these sections, even when empty.

At the top:
`Scan period: ...`

## Best matches
Company | Title | Location | Work mode | Salary | Match | Why | Source | Apply

Only populate when profile_read_status = VERIFIED.
Order Strong before Possible, then older ReceivedAt first unless the profile explicitly requests another order.

Use clickable Apply links when available.
The Why field should use actual scope, differentiators, specialty, domain, or outcomes. Do not use `same title` as the main reason for an experienced candidate when stronger evidence exists.

## Excluded or low priority
Company | Title | Reason | Source

Keep concise and include only jobs actually reviewed during this run.

## Applications detected
Show clear previously untracked application or recruiter-submission evidence handled during this run.

## Responses detected
Company | Title | RespondedAt | Result | Evidence

## No-response candidates
Company | Title | AppliedAt | Days

## Tracker updates
Summarize applied writes or state that no update was needed.

## Manual Tracker fallback
Only when automatic Sheet writes could not be applied. Return exact 13-column TSV rows.

## Human review
List only items needing a decision or manual action, such as:
- ambiguous company/title association
- suspected duplicate with uncertain company identity
- ambiguous recruiter submission language
- conflicting response evidence
- blocked Tracker write

## Source health
List every enabled source with its message count for the target period.
Explicitly list enabled sources with zero messages.
If all major enabled job-alert sources unexpectedly return zero messages, warn that job-alert delivery, sender patterns, or account configuration may need review.

## Diagnostics
Report:
- profile_read_status and profile_version
- Config version
- target period and timezone
- previous and resulting last_successful_scan_date
- tracker_read_status and row-count comparison
- number of messages read per enabled source
- number of all-inbox messages read for reconciliation when enabled
- number of extracted postings before filtering
- link extraction failures
- parsing or classification ambiguities
- automatic Tracker write result
- any skipped module and why

DIAGNOSTIC RULES
- State evidence for anomalies.
- Separate confirmed from suspected.
- Never claim absence when the relevant source was not fully read.
- Never substitute Memory when the private profile is unavailable.
```
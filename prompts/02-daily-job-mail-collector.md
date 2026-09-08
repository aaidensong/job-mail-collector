# Daily Job Mail Collector prompt template

`{{SHEET_REFERENCE}}` is replaced by the bootstrap flow with the user's own Google Sheet reference.

```text
Run my daily Job Mail Collector using this Google Sheet for operational configuration and tracking:
{{SHEET_REFERENCE}}

Do not rely on ChatGPT Memory, custom instructions, old chats, Project files, uploaded files, or assumptions about me.

My career data is stored in the private profile referenced by Config.profile_reference. Read that profile on every run before evaluating job fit.

[1. LOAD OPERATIONAL CONFIGURATION]
Read the Config, Sources, Tracker, and Control tabs.

Require Config.config_version = 3.
Use Config.schedule_timezone for all date and time judgments and output.
Use Config.scan_window to determine the Gmail time window.
Use only Sources rows where Enabled is true.

Before making any Tracker-dependent judgment, verify Tracker read completeness:
1. Count non-empty Tracker data rows using Company as the required field.
2. Compare that count with Control.tracker_data_rows.
3. If they match, tracker_read_status = VERIFIED.
4. If they do not match, retry the Tracker read once using the broadest available Sheet read method.
5. If the second count still does not match, tracker_read_status = INCOMPLETE.

When tracker_read_status = INCOMPLETE:
- continue extracting jobs from Gmail;
- do not claim that a job is absent from the Tracker;
- do not mark historical duplicates as confirmed;
- skip all optional modules that depend on complete Tracker history;
- label deduplication against history as `unverified`;
- report the mismatch in Diagnostics.

[2. LOAD AND VALIDATE PRIVATE CAREER PROFILE]
Read the entire document referenced by Config.profile_reference from my connected Google Drive.
Config.profile_storage_format may be:
- markdown_file
- google_doc_markdown

Treat the retrieved text as Markdown with YAML front matter.
Do not substitute ChatGPT Memory or any other personal context if the profile cannot be read.

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
resume_versions

Require profile_version = 1.
Also read the Markdown sections, especially:
- Professional summary
- Experience highlights
- Measurable outcomes
- Core skills and strengths
- Portfolio or specialty areas
- Role preferences and interpretation notes
- Additional context

Set profile_read_status:
- VERIFIED: document is readable and required YAML can be interpreted reliably
- INVALID: document is readable but materially malformed or missing required schema
- UNAVAILABLE: document cannot be accessed/read

When profile_read_status is not VERIFIED:
- do not classify jobs as Strong, Possible, or Weak;
- do not apply personal hard filters whose source would be the profile;
- do not infer the user's target from task history, Memory, or prior results;
- continue source-health checks and basic job extraction only if useful;
- report PROFILE_INVALID or PROFILE_UNAVAILABLE in Diagnostics;
- do not produce candidate rows in `Paste into Tracker` because personal fit has not been validated.

[3. READ JOB-ALERT EMAILS]
Search Gmail for the configured scan window and every enabled Sources.SenderPattern.
Read messages, not only thread summaries.
If DigestMode is true, open the message body and extract every distinct job posting in the digest.
Do not assume the email subject represents the only job in the message.

For each extracted job, capture when available:
- company
- title
- location
- salary
- work mode
- source
- received timestamp converted to Config.schedule_timezone
- raw application/job URL
- short evidence needed for fit judgment

Do not invent missing company names, titles, salaries, locations, or links.
If a company is represented by an unfamiliar subsidiary or brand name, preserve the email's wording unless the relationship is explicitly stated in the email or linked job page.

[4. EXCLUDE NON-JOB MESSAGES]
Source-specific rules in Sources.LinkRule or Sources.Notes override generic behavior.
Exclude application confirmations, newsletters, marketing messages, career advice, and recruiter content that does not contain an identifiable open role from the candidate-job collection.
Keep application confirmations available for the optional missing-application module if enabled.

[5. NORMALIZE LINKS]
Prefer a stable, directly usable job or application URL.
For LinkedIn, when a job ID is explicit, normalize to:
https://www.linkedin.com/jobs/view/{JOB_ID}/
Remove tracking parameters from normalized LinkedIn URLs.
For other sources, preserve a usable job/application URL from the message or linked page.

If the URL is absent, broken, inaccessible, or cannot be associated with the correct job confidently:
- leave Link blank;
- add `link not extracted` to Notes;
- never reconstruct or guess a URL.

If a link resolves to an obviously expired or removed posting, keep the URL only if useful for identification and mark the job Excluded with reason `posting unavailable`.

[6. DEDUPLICATE WITHIN THIS RUN]
Primary duplicate key: normalized Company + normalized Title.
Use location as a tie-breaker when the same title clearly represents different openings.
If the same posting appears from multiple sources, keep one candidate and join source names with ` + `.
Prefer the cleanest usable application link.

[7. APPLY HARD FILTERS]
Run only when profile_read_status = VERIFIED.

Apply explicit hard rules from the profile, including when relevant:
- excluded_titles
- excluded_domains
- disallowed employment types
- disallowed locations/work models when the profile makes them hard constraints
- sponsorship/work-authorization blockers according to sponsorship_rule
- hard_exclude_keywords
- hard_skill_blockers

Use Markdown interpretation notes when they clarify whether a rule is hard or warning-only.

A hard exclusion must include a short reason based on evidence from the posting/profile.
Do not turn a missing detail into a hard exclusion unless the profile explicitly says missing information is disqualifying.

Management handling:
- if management_roles = unacceptable, exclude people-management roles;
- if review-needed, keep them and add a warning;
- if acceptable, evaluate normally.

[8. SCORE REMAINING JOBS]
Run only when profile_read_status = VERIFIED.

For jobs not hard-excluded, assess fit using both structured YAML and the Markdown career evidence.
Use these dimensions:
- title/seniority fit
- product/domain fit
- skills/experience fit
- evidence of relevant scope or outcomes
- location/work-model fit
- employment/compensation fit when known

Classify each as:
Strong match
Possible match
Weak match

Do not create false precision with a numeric score unless the private profile explicitly requests one.
A Strong match should have clear positive evidence and no major unresolved conflict.
A Possible match may have missing or ambiguous information that needs user review.
A Weak match should not appear in the main shortlist unless there are no stronger jobs; it may be summarized in Excluded/Low-priority output.

[9. COMPARE AGAINST TRACKER]
Only do confirmed historical duplicate checks when tracker_read_status = VERIFIED.

Rules:
- same Company + same Title: treat as historical duplicate; do not add as a new Candidate unless the posting is clearly a materially different requisition and that difference is evidenced.
- same Company + different Title: keep it, but add concise prior-history context when relevant.
- staffing/recruiting agencies are channels, not automatically the employer. Do not deduplicate solely on agency name.

If a current posting is already in Tracker with Status=Excluded because the old posting was unavailable or expired, allow a clearly new requisition to be reconsidered when evidence shows it is a new opening.

[10. OPTIONAL: MISSING APPLICATION DETECTION]
Run only if Config.module_missing_application is enabled AND tracker_read_status = VERIFIED.
Inspect application-confirmation messages in the scan window.
If Company + Title cannot be found in Tracker, create an Applied row and write it directly when Config.auto_application_update is true and write access allows it. Otherwise include it in the manual TSV fallback.
If the confirmation is for a general career-page submission without a title, preserve the email wording and use a non-colliding title such as `Unknown (Career Page)` only when the email truly has no role title.
Do not infer a title from unrelated context.

[11. OPTIONAL: RESPONSE DETECTION]
Run only if Config.module_response_detection is enabled AND tracker_read_status = VERIFIED.
For Tracker rows with Status=Applied, search Gmail for responses received after AppliedAt.
Use company name, recruiter/employer sender clues, job title, and thread context together.
Do not classify generic job alerts or unrelated company mail as a response.

If Config.module_rejection_stage is enabled, estimate RejectionStage only from available evidence:
- ATS automatic
- recruiter screening
- portfolio/hiring-manager review
- after interview
- unknown

If Config.module_ats is enabled, estimate ATS from sender domain only when there is a recognizable platform signature. Otherwise use `unknown`, not a fabricated vendor.

[12. OPTIONAL: NO-RESPONSE CHECK]
Run only if Config.module_no_response is enabled AND tracker_read_status = VERIFIED.
Find Tracker rows with Status=Applied, RespondedAt blank, and elapsed local calendar days >= Config.no_response_days.
Return them as no-response candidates. Do not automatically close them unless the user's configuration explicitly requests auto-closing.

[13. OUTPUT]
Always produce these sections, even when empty:

## Best matches
Company | Title | Location | Work mode | Salary | Match | Why | Source | Apply

Only populate when profile_read_status = VERIFIED.
Order by:
1. Strong match before Possible match
2. within the same match class, older ReceivedAt first unless the private profile explicitly specifies another order

Use clickable links in Apply when available.

## Excluded or low priority
Company | Title | Reason | Source
Keep this concise. Include only jobs actually reviewed during this run.
When the profile is unavailable, include only non-profile exclusions such as obvious non-job messages or unavailable postings, not personal-fit exclusions.

## Missing applications
Only when module is enabled. When auto_application_update is true, apply clear missing-application reconciliation directly to Tracker when write access allows it; otherwise include it in the manual fallback.

## Responses detected
Only when module is enabled. Apply clear RespondedAt or rejection updates directly when auto_application_update is true and write access allows it.

## No-response candidates
Only when module is enabled.

## Tracker updates
First, determine and apply the intended Tracker changes according to the automation rules below.

Preferred mode:
- if Config.auto_candidate_write is true and the Google Drive write action can run with the current task permissions, append new suitable Candidate rows directly to Tracker;
- if Config.auto_application_update is true and evidence is unambiguous, update matching existing rows directly;
- never overwrite user-entered data with a lower-confidence inference;
- after writes, reread the affected rows when possible and report whether the change was applied.

Fallback mode:
- if a required external write cannot proceed because approval is required or the write action is unavailable, do not silently fail;
- return every intended insert or update as a fenced TSV block;
- label the section `Manual Tracker fallback` and state that the automatic write was not applied.

Use exactly 17 columns in this order:
Status	Company	Title	Location	Salary	WorkMode	Notes	Link	ReceivedAt	AppliedAt	RespondedAt	Result	RejectionStage	Channel	ATS	ResumeVersion	Source

For new job candidates:
- Status = Candidate
- leave AppliedAt, RespondedAt, Result, RejectionStage, ATS, ResumeVersion blank unless known from an enabled optional module
- Channel should describe an agency/recruiter only when applicable
- Source should contain the alert platform/source

Sort new Candidate rows by ReceivedAt ascending.
Do not add confirmed historical duplicates or hard-excluded jobs.
Do not create new Candidate rows if profile_read_status is not VERIFIED.

For automatic status reconciliation:
- clear application confirmation for Company + Title -> set Status=Applied and AppliedAt to confirmation received time when AppliedAt is blank;
- clear recruiter/employer response -> fill RespondedAt when blank;
- explicit rejection -> set Status=Closed and Result=Rejected; fill RejectionStage only when enabled and supported by evidence;
- recognizable ATS sender domain -> fill ATS only when module_ats is enabled and the vendor is supported by evidence;
- ambiguous or conflicting evidence -> do not change the row automatically; report it under Human review.

A user may apply without receiving a confirmation email. In that case, do not guess that the application happened. Leave the row as Candidate until the user changes it or reliable evidence appears.

## Human review
List only items that need a user decision or manual action, such as ambiguous company/title matching, an application with no confirmation email, conflicting status evidence, or a blocked Tracker write.

## Diagnostics
Report:
- profile_read_status, profile_version, and storage format
- automatic Tracker write result: applied, not needed, approval required, or unavailable
- Config version
- scan window and timezone
- number of Gmail messages read per enabled source
- sources with zero messages
- number of extracted postings before filtering
- tracker_read_status and row-count comparison
- link extraction failures
- parsing ambiguities
- any skipped module and why

DIAGNOSTIC RULES
- State the evidence for anomalies.
- Separate `confirmed` from `suspected`.
- Never claim data is absent when the relevant source was not fully read.
- Never substitute personal information from Memory when the private profile is unavailable.
- If all major configured job-alert sources unexpectedly return zero messages, flag `job-alert delivery or account configuration may need review`.
```

# Bootstrap prompt

Run this once in a normal ChatGPT conversation after connecting Gmail and Google Drive in ChatGPT.

```text
You are setting up Job Mail Collector for me inside ChatGPT.

This is an interactive setup. Do not tell me to edit configuration files, move generated files, run terminal commands, or manually create a Scheduled Task unless a capability is unavailable in my account.

Your responsibilities in this setup conversation are to:
1. confirm that Gmail and Google Drive are available to this conversation;
2. ask me the information needed to judge job fit;
3. create my private career profile in my connected Google Drive;
4. discover or collect the Gmail job-alert sources I want monitored;
5. create my Google Sheet tracker in my connected Google Drive;
6. configure automatic Tracker writing when supported;
7. create a recurring ChatGPT Scheduled Task at the time I choose;
8. run a test before declaring setup complete.

Do not use ChatGPT Memory, custom instructions, old chats, Project files, uploaded files, or assumptions about me as substitutes for the answers collected in this setup.

[0. APP ACCESS CHECK]
Before asking profile questions, verify that you can use the user's connected Gmail and Google Drive in this conversation.

If Gmail is unavailable, tell the user exactly that Gmail must be connected in ChatGPT Settings > Apps or Settings > Plugins, depending on the interface available to the account.

If Google Drive is unavailable, give the same instruction for Google Drive.

Do not continue to final setup until both resources can be accessed.

[1. ONBOARDING QUESTIONS]
Ask the following in logical groups inside this chat. The user answers in this same conversation. There is no separate questionnaire for the user to find.

Skip questions the user has already answered.

A. Schedule
- What time should the daily scan run?
- What time zone should all date calculations use?
- What email window should each run scan? Default: previous local calendar day.

B. Target roles
- Primary job titles actively wanted.
- Adjacent titles worth considering.
- Target seniority levels.
- Titles or seniority levels never wanted.
- Whether people-management roles are acceptable, unacceptable, or review-needed.

C. Career evidence
Ask for enough factual evidence to distinguish real matches from title-only matches:
- current or recent role family;
- approximate years of relevant experience when useful;
- products, domains, or business models with the strongest relevant experience;
- strongest skills and responsibilities;
- measurable outcomes or concrete evidence that should strengthen a match;
- specialty areas or portfolio focus that should matter when matching.

A full chronological resume is optional. Do not request irrelevant personal information.

D. Product and domain fit
- Preferred product types, industries, and domains.
- Domains to exclude.
- Strongest experience or skills that should increase fit.
- Skills or experience gaps that should be hard blockers, if any.

E. Location and work model
- Target countries, cities, or regions.
- Remote, hybrid, and on-site preferences.
- Commute or relocation constraints when relevant.

F. Employment constraints
- Full-time, contract, part-time, internship preferences.
- Work authorization and sponsorship situation.
- Whether sponsorship requirements are a hard filter, warning only, or ignored.
- Minimum compensation if the user wants compensation filtering.

G. Search behavior
- Company types to prefer or avoid.
- Agency and recruiter handling rules.
- Keywords that always exclude a posting.
- Keywords that create a warning but do not exclude.
- Languages relevant to work.
- Resume-version labels used for different role families, if any.

H. Existing application history
- Ask whether the user has an existing tracker to import.
- Use it only if the user explicitly provides or authorizes it.
- Otherwise start with an empty Tracker.

I. Automation modules
Ask whether to enable:
- automatic Candidate writes to Tracker
- missing application detection from confirmation emails
- automatic application status updates
- employer/recruiter response detection
- rejection-stage estimation
- ATS estimation
- no-response-after-N-days detection

Recommended defaults:
- automatic Candidate writes: enabled
- missing application detection: enabled
- automatic application status updates: enabled
- response detection: enabled
- rejection-stage estimation: enabled
- ATS estimation: enabled
- no-response detection: enabled
- no-response days: 14

Explain that external write actions can require approval depending on the user's account or workspace. If unattended writes are blocked, the Scheduled Task will return TSV as a fallback instead of silently failing.

J. Job-alert sources
Offer two choices:
1. Auto-discover: search the last 30 days of the connected Gmail for likely job-alert senders and show a proposed source list for confirmation.
2. Manual: ask for sender addresses or domains.

Never enable a sender the user has not confirmed.
For digest sources, record that one email can contain multiple jobs.

[2. CREATE PRIVATE CAREER PROFILE]
After career-related answers are complete, create a private profile owned by the user.

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

## Experience highlights
Record the roles, products, domains, responsibilities, and scope needed for job matching.

## Measurable outcomes
Record only outcomes or metrics actually provided.

## Core skills and strengths
Record strengths that should materially increase job fit.

## Portfolio or specialty areas
Record specialty areas that should influence matching.

## Role preferences and interpretation notes
Capture nuance that does not fit cleanly in the YAML fields.

## Additional context
Add only stable career context that materially improves matching.

Before saving:
- verify the YAML field names exactly match the schema;
- verify the YAML can be interpreted reliably;
- verify no career fact was invented;
- verify hard exclusions and warning-only preferences are distinct;
- verify the Markdown body contains enough evidence to judge fit.

[3. CREATE GOOGLE SHEET]
Create one Google Sheet named `Job_Mail_Collector` unless the user chooses another name.

The user should not have to create or move the Sheet manually.

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

[4. VERIFY TRACKER WRITE CAPABILITY]
If auto_candidate_write or auto_application_update is enabled, test whether a supported Google Drive action can update the Sheet with the user's current permissions.

Do not add fake job data to Tracker to perform this test.
Use a harmless supported method such as writing and restoring a dedicated Config test value when possible.

Set the operating behavior:
- AUTO_WRITE_AVAILABLE: scheduled runs should write directly to Tracker.
- APPROVAL_OR_WRITE_UNAVAILABLE: scheduled runs should return exact intended changes as TSV and clearly report that the write was not applied.

Do not claim automatic writing works unless the capability was actually verified.

[5. CREATE THE SCHEDULED TASK]
Use the full prompt in `02-daily-job-mail-collector.md`, replacing {{SHEET_REFERENCE}} with the exact Sheet created during setup.

Create a recurring ChatGPT Scheduled Task at the time and time zone selected by the user.

The task runs inside ChatGPT. The user does not need to run a program, keep a browser tab open, or manually trigger the daily workflow.

If the interface requires the user to approve creation of the task, ask for that approval at the point the product requires it.

Do not embed the user's detailed career profile inside the task prompt. The recurring task reads the profile reference from the Sheet every time.

[6. TEST RUN]
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
- automatic Tracker writing is available when enabled, or the TSV fallback is correctly configured.

If the test window has no enabled-source email, use the nearest recent date that contains one.

[7. SETUP COMPLETION OUTPUT]
Return:
1. profile name and exact reference;
2. profile storage format;
3. Sheet name and link/reference;
4. Scheduled Task name, run time, and timezone;
5. enabled Gmail sources;
6. concise matching rules without reproducing the full career history;
7. enabled automation modules;
8. Tracker write mode: AUTO_WRITE_AVAILABLE or APPROVAL_OR_WRITE_UNAVAILABLE;
9. test result;
10. any permission, parsing, profile, Gmail, Sheet, or scheduling issue still requiring user action.

Do not declare setup complete if Gmail, the private profile, the Sheet, or Scheduled Task creation failed.
```

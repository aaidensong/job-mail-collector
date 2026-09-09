# Google Sheet schema

Default file name: `Job_Mail_Collector`

Career history and job-fit rules do not live in this Sheet. They live in the private profile document described in `profile-file.md`.

## Config

| Column | Name | Purpose |
|---|---|---|
| A | Key | Stable operational configuration key |
| B | Value | User/system value |
| C | Notes | Explanation or edge-case handling |

Required keys:

- `config_version` = `4`
- `profile_reference`
- `profile_storage_format` = `markdown_file` or `google_doc_markdown`
- `schedule_time`
- `schedule_timezone`
- `scan_window` = `catch_up_from_last_successful_scan`
- `auto_candidate_write` = `true` or `false`
- `auto_application_update` = `true` or `false`
- `module_missing_application`
- `module_response_detection`
- `module_no_response`
- `no_response_days`
- `write_fallback` = `tsv`

Do not duplicate the user's career history or target-role rules into Config.

## Sources

| Column | Name | Purpose |
|---|---|---|
| A | Source | Human-readable source name |
| B | SenderPattern | Gmail sender address or domain/pattern |
| C | Enabled | true/false |
| D | DigestMode | true if one message can contain multiple jobs |
| E | LinkRule | Source-specific link normalization instruction |
| F | Notes | Parsing and message-classification notes |

Do not hardcode every provider in the public prompt. Let each user confirm the senders that actually reach their inbox.

A sender can produce more than one message type. Message classification must consider sender, subject, and body together. For example, the same sender can produce both job alerts and application confirmations.

## Tracker

Exactly 13 columns:

| # | Column | Name |
|---:|---|---|
| 1 | A | Status |
| 2 | B | Company |
| 3 | C | Title |
| 4 | D | Location |
| 5 | E | Salary |
| 6 | F | WorkMode |
| 7 | G | Notes |
| 8 | H | Link |
| 9 | I | ReceivedAt |
| 10 | J | AppliedAt |
| 11 | K | RespondedAt |
| 12 | L | Result |
| 13 | M | Source |

Status values:
- Candidate
- Applied
- Closed
- Excluded

`Source` is the single provenance field for the opportunity or application evidence, for example `LinkedIn`, `Indeed`, `Randstad email`, or `Recruiter email`. Do not maintain separate ATS, resume-version, or channel columns. If agency or recruiter context matters for a specific row, put that detail in `Notes`.

`ReceivedAt` is retained so users can tell how old a discovered opportunity is even when they review or apply several days later.

The daily task does not rely on formula columns inside Tracker.

## Control

| Metric | Value | Notes |
|---|---|---|
| config_version | 4 | schema/config compatibility |
| tracker_data_rows | formula | non-empty Company rows in Tracker |
| candidate_count | formula | Status=Candidate |
| applied_count | formula | Status=Applied |
| closed_count | formula | Status=Closed |
| excluded_count | formula | Status=Excluded |
| last_successful_scan_date | date or blank | last local calendar date fully processed by the scheduled workflow |

`tracker_data_rows` is the critical completeness check. The scheduled task compares the count it actually read with this value before it makes absence, duplicate, response, or untracked-application judgments.

`last_successful_scan_date` provides missed-run recovery. On each scheduled run, the task should process every local calendar day after this value through the previous local calendar day. On the first run, use only the previous local calendar day. Update the value only after the full target period was processed successfully enough to produce the normal output or an explicit write fallback.
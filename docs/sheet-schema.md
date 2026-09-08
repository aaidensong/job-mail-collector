# Google Sheet schema

Default file name: `Job_Mail_Collector`

Career history and job-fit rules do not live in this Sheet. They live in the separate private profile document described in `profile-file.md`.

## Config

| Column | Name | Purpose |
|---|---|---|
| A | Key | Stable operational configuration key |
| B | Value | User/system value |
| C | Notes | Explanation or edge-case handling |

Required keys:

- `config_version` = `3`
- `profile_reference`
- `profile_storage_format` = `markdown_file` or `google_doc_markdown`
- `schedule_time`
- `schedule_timezone`
- `scan_window`
- `auto_candidate_write` = `true` or `false`
- `auto_application_update` = `true` or `false`
- `module_missing_application`
- `module_response_detection`
- `module_no_response`
- `no_response_days`
- `module_rejection_stage`
- `module_ats`
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
| F | Notes | Other parsing rules |

Do not hardcode every provider in the public prompt. Let each user confirm the senders that actually reach their inbox.

## Tracker

Exactly 17 columns:

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
| 13 | M | RejectionStage |
| 14 | N | Channel |
| 15 | O | ATS |
| 16 | P | ResumeVersion |
| 17 | Q | Source |

Status values:
- Candidate
- Applied
- Closed
- Excluded

The daily task does not rely on formula columns inside Tracker.

## Control

| Metric | Value | Notes |
|---|---|---|
| config_version | 3 | schema/config compatibility |
| tracker_data_rows | formula | non-empty Company rows in Tracker |
| candidate_count | formula | Status=Candidate |
| applied_count | formula | Status=Applied |
| closed_count | formula | Status=Closed |
| excluded_count | formula | Status=Excluded |

`tracker_data_rows` is the critical completeness check. The scheduled task compares the count it actually read with this value before it makes absence, duplicate, response, or untracked-application judgments.

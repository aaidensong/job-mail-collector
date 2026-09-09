# Troubleshooting

## The private profile cannot be read

Symptoms:
- `PROFILE_UNAVAILABLE`
- the scheduled run can see the Sheet but cannot open `Config.profile_reference`

Checks:
- confirm Google Drive is connected to the same account that owns or can access the profile;
- confirm the stored profile reference still points to the intended file/document;
- confirm the profile is not only a Project upload or task attachment;
- if a raw `.md` file is not readable in the current environment, use the Google Doc Markdown fallback and update `Config.profile_storage_format` and `Config.profile_reference`.

Do not fall back to ChatGPT Memory or infer the profile from previous task results.

## The profile is marked invalid

Symptoms:
- `PROFILE_INVALID`
- YAML fields cannot be parsed reliably

Checks:
- keep the `---` YAML delimiters;
- preserve the required field names from `profiles/profile.template.md`;
- ensure list fields remain valid YAML lists;
- keep `profile_version: 2` unless the repository schema is intentionally upgraded.

Use `prompts/04-update-profile.md` to repair the file while preserving existing factual content.

## Tracker count mismatch

If the rows read from Tracker do not equal `Control.tracker_data_rows`, the task must mark history as incomplete.

It may still parse current Gmail job-alert messages for diagnostics, but it must not claim that a job or application is absent from Tracker, confirm historical duplicates, reconcile application state, write new Candidate rows, or advance `last_successful_scan_date`.

## A scheduled run was skipped

The workflow uses `Control.last_successful_scan_date` instead of assuming that yesterday was always processed.

On the next run, it should process every local calendar day after the last successful date through yesterday.

If the date did not advance after a failed run, that is expected. The next successful run should catch up automatically.

Do not use the newest Tracker `ReceivedAt` value as a substitute for the successful-scan marker. A day can be processed successfully even when no candidate was added.

## No job alerts found

Check Source health and Diagnostics.

Possible causes:
- the sender pattern is wrong;
- the source stopped sending alerts;
- Gmail is connected to a different account;
- the calculated scan period or timezone is wrong;
- messages are being delivered under a new sender address.

Every enabled source with zero messages should be listed.
If all major sources unexpectedly return zero messages, review alert delivery, sender patterns, and account configuration.

## A sender produced the wrong kind of message

A sender address does not define one permanent message type.

For example, one sender may send both job recommendations and application confirmations.

The workflow should classify messages from sender + subject + body before deciding how to use them.

If classification is wrong, update `Sources.Notes` with stable content clues rather than creating a separate sender solely to represent the message type.

## Links are missing

A blank link is preferable to a fabricated link.

Check whether:
- the digest contains a direct job URL;
- the source changed its email markup;
- the URL is only a tracking redirect that cannot be safely associated with a posting;
- the job expired or was removed.

Update the corresponding Sources.LinkRule only when a stable parsing rule is known.

## A recruiter said they submitted me, but the Tracker was not updated

Clear recruiter language can count as application evidence when it explicitly states that the user's application, profile, or resume was submitted or forwarded for a specific company and role.

Ambiguous statements such as `I may submit you` or `I can send your profile` should not trigger an automatic status change.

If the evidence is ambiguous, the workflow should put it in Human review.

## Company names look different for what may be the same job

Do not guess a parent company from an unfamiliar subsidiary, brand, or legal entity name.

Preserve the source wording.
If the records look related but identity cannot be verified confidently, mark a suspected duplicate for Human review instead of merging automatically.

## Scheduled task cannot use a connected app

Connected-app availability and supported actions depend on plan, workspace settings, permissions, and app capabilities.

Verify the user connected their own Gmail and Google Drive. A shared task does not inherit the original creator's credentials or files.

Durable matching information must live in the private Drive profile referenced by Config.profile_reference.

## Tracker did not update automatically

Symptoms:
- the Scheduled Task found suitable jobs but returned `Manual Tracker fallback`;
- Diagnostics says `approval required` or `write unavailable`.

Checks:
- confirm Google Drive is connected to the account that owns the Tracker;
- confirm the connected Google account has edit access to the Sheet;
- confirm the relevant Google Drive write actions are available;
- review whether the workspace requires approval for external data changes.

The workflow must not claim a write succeeded when it did not. When an unattended write is blocked, use the exact 13-column TSV fallback.

## I applied but the row still says Candidate

Job Mail Collector normally learns that an application was submitted from clear Gmail evidence such as an application-confirmation email or an explicit recruiter-submission message.

If neither exists, there may be no reliable evidence that the application happened. In that case, manually change the Tracker row to `Applied` and enter `AppliedAt` if desired.

Do not infer an application merely because the user opened an application link.

## Where are ATS, ResumeVersion, Channel, and RejectionStage?

They are intentionally not part of the current Tracker schema.

The core workflow keeps only fields that materially help users find, review, apply to, and track opportunities.

If recruiter or agency context matters for a specific application, put that detail in Notes. `Source` is the single provenance field.
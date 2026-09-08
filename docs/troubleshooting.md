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
- keep `profile_version: 1` unless the repository schema is intentionally upgraded.

Use `prompts/04-update-profile.md` to repair the file while preserving existing factual content.

## Tracker count mismatch

If the rows read from Tracker do not equal `Control.tracker_data_rows`, the task must mark history as incomplete.

It may still parse current Gmail job alerts, but it must not claim that a job or application is absent from Tracker and must skip history-dependent optional modules.

## No job alerts found

Check Diagnostics by source.

Possible causes:
- the sender pattern is wrong;
- the source stopped sending alerts;
- Gmail is connected to a different account;
- the scan window or timezone is wrong;
- messages are being delivered under a new sender address.

Use setup/source discovery again and confirm new senders before enabling them.

## Links are missing

A blank link is preferable to a fabricated link.

Check whether:
- the digest contains a direct job URL;
- the source changed its email markup;
- the URL is only a tracking redirect that cannot be safely associated with a posting;
- the job has expired or been removed.

Update the corresponding Sources.LinkRule only when a stable parsing rule is known.

## Scheduled task cannot use a connected app

Connected-app availability and supported actions depend on plan, workspace settings, permissions, and app capabilities.

Verify the user connected their own Gmail and Google Drive. A shared task does not inherit the original creator's credentials or files.

Do not depend on Project uploads, shared-task history, or a file attached only to the setup chat. Durable matching information must live in the private Drive profile referenced by Config.profile_reference.

## Tracker did not update automatically

Symptoms:
- the Scheduled Task found suitable jobs but returned `Manual Tracker fallback`;
- Diagnostics says `approval required` or `write unavailable`.

Checks:
- confirm Google Drive is connected to the account that owns the Tracker;
- confirm the connected Google account has edit access to the Sheet;
- confirm the relevant Google Drive write actions are available for the ChatGPT account or workspace;
- review whether the workspace requires approval for external data changes.

The workflow must not claim that a Tracker write succeeded when it did not. When an unattended write is blocked, use the returned TSV as the fallback.

## I applied but the row still says Candidate

Job Mail Collector normally learns that an application was submitted from clear Gmail evidence such as an application-confirmation email.

If the employer or ATS sends no confirmation email, there may be no reliable evidence that the application happened. In that case, manually change the Tracker row to `Applied` and enter `AppliedAt` if desired.

Do not configure the workflow to infer an application merely because the user opened an application link.

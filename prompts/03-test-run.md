# Test-run prompt

Use this after bootstrap or after changing the profile, sources, or schema.

```text
Run Job Mail Collector in TEST MODE only.

Use the configured Job Mail Collector Google Sheet, the private profile referenced by Config.profile_reference, and the configured Gmail sources.
Do not create, edit, delete, or append Tracker rows.
Do not change the recurring schedule.

Test the nearest recent 24-hour window that contains at least one enabled-source email. If yesterday contains enabled-source mail, use yesterday.

Return:
1. Config.config_version and whether all required Config values were readable;
2. Config.profile_reference and profile_storage_format;
3. whether the private profile was readable;
4. profile_version and whether all required YAML keys were interpretable;
5. whether the Markdown career-evidence sections, including Search interpretation and Leadership experience when present, were readable;
6. enabled Sources and message count for each;
7. one example of a digest email expanded into individual jobs, if available;
8. Tracker row count read vs Control.tracker_data_rows;
9. up to three extracted jobs with their parsed application URLs;
10. hard-filter result and match class for those examples when profile validation passes;
11. for any example whose title or career-level label differs from the user's main target, whether the decision was based on actual responsibilities and scope rather than exact title equality;
12. whether the previously verified automatic Tracker write capability is still available when automatic writing is enabled, or whether the TSV fallback would be required;
13. all permissions, profile, parsing, source, or completeness failures.

Fit-based validation rules:
- `primary_titles` and `target_seniority` are anchors, not automatic whitelists;
- a job must not fail only because its exact title or level is absent from those fields;
- Staff, Lead, or another nearby title should remain eligible when the actual responsibilities and scope are supported by the user's evidence, unless explicitly excluded;
- a superficially similar title can still be low-fit when its actual requirements are materially unsupported;
- people-management requirements must be distinguished from project leadership, mentoring, or cross-functional leadership.

Pass criteria:
- private profile readable and valid
- Gmail readable
- Sheet readable
- Tracker completeness verified, or an explicit INCOMPLETE diagnostic is produced without unsupported absence claims
- enabled source search works
- digest extraction works when a digest exists
- no fabricated application link
- no plausible job is excluded solely because its exact title or career-level label differs from the main target
- output schema is exactly 17 columns

Do not say the setup passed if the profile, Gmail, or Sheet required permission failed.
```

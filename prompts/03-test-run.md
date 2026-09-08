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
5. whether the Markdown career-evidence sections were readable;
6. enabled Sources and message count for each;
7. one example of a digest email expanded into individual jobs, if available;
8. Tracker row count read vs Control.tracker_data_rows;
9. up to three extracted jobs with their parsed application URLs;
10. hard-filter result and match class for those examples when profile validation passes;
11. whether the previously verified automatic Tracker write capability is still available when automatic writing is enabled, or whether the TSV fallback would be required;
12. all permissions, profile, parsing, source, or completeness failures.

Pass criteria:
- private profile readable and valid
- Gmail readable
- Sheet readable
- Tracker completeness verified, or an explicit INCOMPLETE diagnostic is produced without unsupported absence claims
- enabled source search works
- digest extraction works when a digest exists
- no fabricated application link
- output schema is exactly 17 columns

Do not say the setup passed if the profile, Gmail, or Sheet required permission failed.
```

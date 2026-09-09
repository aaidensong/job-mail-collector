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
5. whether the Markdown career-evidence sections were readable, including Search interpretation and Differentiators and scope when present;
6. whether an experienced user's profile contains enough evidence beyond title and years to explain meaningful fit differences;
7. enabled Sources and message count for each;
8. one example of a digest email expanded into individual jobs, if available;
9. Tracker row count read vs Control.tracker_data_rows;
10. up to three extracted jobs with their parsed application URLs;
11. hard-filter result and match class for those examples when profile validation passes;
12. for every Strong or Possible example, the specific profile evidence used to justify the match;
13. whether the matcher avoided rejecting plausible roles solely because the title or career-level label differs from the user's main target;
14. whether the previously verified automatic Tracker write capability is still available when automatic writing is enabled, or whether the TSV fallback would be required;
15. all permissions, profile, parsing, source, or completeness failures.

Pass criteria:
- private profile readable and valid
- Gmail readable
- Sheet readable
- Tracker completeness verified, or an explicit INCOMPLETE diagnostic is produced without unsupported absence claims
- enabled source search works
- digest extraction works when a digest exists
- no fabricated application link
- output schema is exactly 17 columns
- plausible nearby titles are evaluated by actual scope, not title label alone
- Strong matches for experienced users use at least one meaningful evidence signal beyond exact title similarity when that evidence is available

Do not say the setup passed if the profile, Gmail, or Sheet required permission failed.
```

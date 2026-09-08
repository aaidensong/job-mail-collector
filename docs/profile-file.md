# Private career profile file

Job Mail Collector separates personal career information from operational tracking data.

## Why a separate Markdown profile

The user's career history, strengths, target roles, constraints, and matching preferences belong in a portable private document rather than inside the public scheduled-task instructions.

The profile uses Markdown with YAML front matter:

- YAML stores stable fields the scheduled workflow should interpret consistently.
- Markdown sections store richer career evidence and interpretation that do not fit cleanly into a flat table.
- The file can be reviewed and edited without touching the scheduled prompt.
- A filled-in profile should remain private and should not be committed to the public repository.

Template: `profiles/profile.template.md`

## Structured fields are anchors, not always hard limits

Fields such as `primary_titles`, `adjacent_titles`, and `target_seniority` help the matcher understand the user's main direction.

They should not automatically become exact-title or exact-level whitelists.

For example, a profile can say that `Senior Product Designer` is the main target while the Markdown `Search interpretation` section explains that Staff or Lead roles should still be considered when the actual responsibilities and scope are supported by the user's experience.

Hard exclusions should be explicit, using fields such as:

- `excluded_titles`
- `excluded_domains`
- `hard_skill_blockers`
- `hard_exclude_keywords`
- `management_roles`
- clear natural-language hard constraints in `Role preferences and interpretation notes`

## Search interpretation

The Markdown profile should preserve nuance inferred from the user's natural-language onboarding answers.

Useful examples:

- main role direction;
- nearby role families to consider when actual fit is strong;
- broader career levels that should remain open when scope is supported;
- explicit unwanted roles or levels;
- whether Lead roles are acceptable only when they remain individual-contributor roles;
- whether UX/UI roles are acceptable only for digital product work;
- any other context needed to avoid overly literal title matching.

The user does not need to manually fill these categories. ChatGPT should infer and summarize them from the onboarding conversation, then let the user correct the interpretation.

## Career evidence matters more than labels

The Markdown body should contain enough evidence to compare the user's actual experience with a posting's real requirements.

Useful evidence includes:

- responsibilities and ownership;
- product and domain experience;
- measurable outcomes;
- scope of decisions;
- cross-functional collaboration;
- project or process leadership;
- direct people-management evidence when it exists;
- specialty areas and relevant skills.

Do not infer direct people management from project leadership, mentoring, or cross-functional influence alone.

## Storage

Preferred:

1. Create a private file named `Job_Mail_Collector_Profile.md` in the user's connected Google Drive when the environment supports creating and reading Markdown files directly.
2. Record its exact Drive reference in `Config.profile_reference`.

Compatibility fallback:

If direct Markdown-file creation or reading is unavailable, create a private Google Doc named `Job_Mail_Collector_Profile`, paste the exact Markdown content into the document without changing the structure, and set:

- `Config.profile_storage_format = google_doc_markdown`
- `Config.profile_reference = <exact Google Doc reference>`

The scheduled workflow treats both representations identically after reading the text.

## Source of truth

The private profile document is the source of truth for career and matching information.

The Google Sheet must not duplicate career history or matching rules. It stores only operational configuration, sources, tracker history, and validation metrics.

## Updating the profile

Use `prompts/04-update-profile.md` or edit the private profile document directly while preserving the YAML field names.

Users can describe changes in natural language. They do not need to manually decide which YAML field should change.

If the YAML becomes malformed or the profile becomes too incomplete to interpret reliably, the daily workflow must stop fit classification and report `PROFILE_INVALID` rather than guessing.

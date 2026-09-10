# Private career profile file

Job Search Collector separates personal career information from operational tracking data.

## Why a separate Markdown profile

The user's career history, strengths, target roles, constraints, and matching preferences belong in a portable private document rather than inside the public scheduled-task instructions.

The profile uses Markdown with YAML front matter:

- YAML stores stable fields the scheduled workflow should interpret consistently.
- Markdown stores richer career evidence, including actual ownership, differentiators, and interpretation notes.
- The file can be reviewed and edited without touching the daily task prompt.
- A filled-in profile should remain private and should not be committed to the public repository.

Template: `profiles/profile.template.md`

Current schema: `profile_version = 2`.

Version 2 intentionally removes resume-version tracking. The core profile keeps only information that materially improves job matching.

## Storage

Preferred:

1. Create a private file named `Job_Search_Collector_Profile.md` in the user's connected Google Drive when the environment supports creating and reading Markdown files directly.
2. Record its exact Drive reference in `Config.profile_reference`.

Compatibility fallback:

If direct Markdown-file creation or reading is unavailable, create a private Google Doc named `Job_Search_Collector_Profile`, paste the exact Markdown content into the document without changing the structure, and set:

- `Config.profile_storage_format = google_doc_markdown`
- `Config.profile_reference = <exact Google Doc reference>`

The scheduled workflow treats both representations identically after reading the text.

## Source of truth

The private profile document is the source of truth for career and matching information.

The Google Sheet must not duplicate career history or matching rules. It stores only operational configuration, sources, Tracker history, and validation metrics.

## What should be rich, not minimal

Tracker fields are intentionally minimal, but the career profile should contain enough evidence to make useful distinctions between superficially similar jobs.

For experienced users, the profile should capture when available:

- actual end-to-end ownership
- recurring problem types
- measurable or observable impact
- decision-making under ambiguity
- cross-functional influence
- leadership or mentoring
- systems or process improvement
- specialty or differentiating expertise

A title and years of experience alone are usually not enough for high-quality matching.

## Updating the profile

Use `prompts/04-update-profile.md` or edit the private profile document directly while preserving the YAML field names.

If the YAML becomes malformed or required sections disappear, the daily workflow must stop fit classification and report `PROFILE_INVALID` rather than guessing.
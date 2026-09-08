# Update career profile prompt

Use this when your target roles, experience, constraints, or preferences change.

```text
Update my Job Mail Collector career profile.

Use the private profile document referenced by Config.profile_reference in my Job_Mail_Collector Sheet.
Do not change Sources, Tracker, Control, or the recurring schedule unless I explicitly request it.

First read the entire current profile, including YAML front matter and Markdown sections.
Then ask only for information needed to make the requested change. Do not make me re-enter unchanged information.

Preserve these YAML keys exactly:
profile_version
primary_titles
adjacent_titles
target_seniority
excluded_titles
management_roles
preferred_domains
excluded_domains
strong_skills
hard_skill_blockers
target_locations
work_models
employment_types
work_authorization
sponsorship_rule
minimum_compensation
preferred_company_types
excluded_company_types
hard_exclude_keywords
warning_keywords
languages
resume_versions

After editing:
1. preserve valid YAML syntax;
2. preserve relevant existing career evidence unless it conflicts with my requested change;
3. do not add facts I did not provide or that are not already present in the profile;
4. write the complete updated profile back to the same private profile document;
5. report a concise change summary.

Do not copy my private profile content into a shared scheduled-task instruction.
```

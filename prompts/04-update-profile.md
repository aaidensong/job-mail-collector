# Update career profile prompt

Use this when your target roles, experience, constraints, or preferences change.

```text
Update my Job Mail Collector career profile.

Use the private profile document referenced by Config.profile_reference in my Job_Mail_Collector Sheet.
Do not change Sources, Tracker, Control, or the recurring schedule unless I explicitly request it.

First read the entire current profile, including YAML front matter and Markdown sections.

This is a conversation, not a form.

I may describe changes in natural language and may mention several changes in one message. Extract every relevant change and update all affected fields and interpretation notes.

Do not make me translate my request into YAML or internal field names.
Do not make me re-enter unchanged information.
Ask a follow-up question only when an ambiguity could materially change job matching.

Do not label normal questions as Required or mandatory.
If you ask an optional preference question, clearly say that I can leave it unanswered if it is not useful.
When an answer may be difficult, include a natural-language example without requiring me to copy the format.

Important matching rule:
- `primary_titles` and `target_seniority` are matching anchors, not automatic whitelists;
- nearby titles or broader levels can remain eligible when the actual responsibilities and scope fit my evidence;
- hard exclusions must be explicit;
- if my natural-language change affects this nuance, update `Search interpretation` and `Role preferences and interpretation notes` as well as any structured fields.

Example request:
`Senior Product Designer is still my main target, but I am okay with Staff or Lead when the actual work fits. I still do not want Manager roles.`

Interpret that as a fit-based search preference. Do not require every possible Staff or Lead title to be enumerated manually.

If I add new experience, do not record only the new title. When useful, capture the scope, impact, recurring problem type, or differentiator that makes the experience relevant to matching.

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

Preserve and update these Markdown sections when relevant:
- Professional summary
- Search interpretation
- Differentiators and scope
- Experience highlights
- Measurable outcomes
- Core skills and strengths
- Portfolio or specialty areas
- Leadership experience
- Role preferences and interpretation notes
- Additional context

Keep leadership and direct people management distinct.
Do not infer direct reports from project leadership, mentoring, design direction, or cross-functional influence.

After editing:
1. preserve valid YAML syntax;
2. preserve relevant existing career evidence unless it conflicts with my requested change;
3. do not add facts I did not provide or that are not already present in the profile;
4. ensure title and career-level anchors are not accidentally converted into hard whitelists;
5. preserve or improve meaningful differentiators when new evidence supports them;
6. write the complete updated profile back to the same private profile document;
7. report a concise change summary in plain language.

Do not copy my private profile content into a shared scheduled-task instruction.
```

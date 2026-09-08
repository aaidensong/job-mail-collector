# Matching rules

## Core principle

Judge the actual job, not the title label alone.

A user's stated target title or career level is an anchor for matching, not automatically a whitelist. If the posting's responsibilities, required experience, product context, and scope are supported by the user's career evidence, the role can still be a valid match even when the title uses a different label such as Staff, Lead, Principal, UX/UI, Growth, or another nearby title.

Only explicit hard exclusions should close that door.

Examples:

- A user mainly targeting `Senior Product Designer` can still receive a `Staff Product Designer` or `Lead Product Designer` role when the actual scope is supported by their experience.
- A role should not be excluded merely because `Staff` or `Lead` was not explicitly listed during onboarding.
- A `Design Manager` role can be excluded when the user explicitly does not want people-management roles.
- A role whose title sounds close but whose required experience is materially unsupported should be downgraded or excluded for that evidence-based reason, not for the title label by itself.

## Decision order

1. Read and validate the private career profile.
2. Parse the posting.
3. Apply explicit hard exclusions.
4. Deduplicate within the current run.
5. Compare with history when Tracker completeness is verified.
6. Evaluate actual fit using career evidence and job scope.
7. Extract and validate the best usable application link.
8. Produce shortlist and Tracker rows.

## Hard exclusions

Use only explicit profile rules or unambiguous posting facts.

Examples:

- title family explicitly listed in `excluded_titles`
- internship when internships are explicitly disallowed
- location outside an allowed region when location is a hard rule
- required sponsorship mismatch when configured as hard
- explicitly excluded domain
- a user-defined blocking keyword
- a required skill listed as a hard blocker when the posting clearly requires it
- people-management responsibility when `management_roles = unacceptable`

Missing information is not automatically a hard exclusion.

`target_seniority` is not a hard exclusion list. Treat it as a reference point unless the profile explicitly says a level must be excluded.

## Fit dimensions

### Role scope and responsibilities

Compare what the person would actually do with the user's demonstrated responsibilities and experience.

This dimension matters more than exact title wording.

### Career level and scope

Compare required years, ownership, decision-making scope, leadership expectations, strategic responsibility, and people-management requirements with the user's evidence.

Do not assume that title labels such as Senior, Staff, Lead, Principal, Manager, or Director mean the same thing at every company.

### Domain

Is the product or industry close to the user's experience or stated target direction?

### Skills and evidence

Does the posting emphasize strengths supported by the private career profile, including concrete responsibilities, outcomes, leadership evidence, or specialty areas?

### Location and work model

Does the stated location and remote, hybrid, or on-site model match the user's constraints and preferences?

### Employment and compensation

Is the employment type acceptable, and does known compensation meet any explicit rule?

## Interpreting target titles

Use three conceptual buckets internally. The user does not need to fill these out as a form.

### Core target

The main role family or direction the user is actively seeking.

Usually represented by `primary_titles`.

### Consider if fit

Nearby titles, different title labels, or broader career levels that should still be evaluated when the actual job scope fits the user's evidence.

This can be represented by `adjacent_titles` plus natural-language notes in the profile.

Do not require every plausible title to be enumerated in advance.

### Hard exclude

Roles the user explicitly does not want regardless of otherwise positive fit.

Usually represented by `excluded_titles`, management rules, excluded domains, hard blockers, or explicit interpretation notes.

## Stretch roles

A role that appears somewhat above or outside the user's usual title should not be rejected automatically.

If the user's evidence supports much of the required scope:

- keep it as Strong or Possible depending on the evidence;
- mention the scope difference briefly when useful;
- let the user decide whether to apply.

If the posting requires materially unsupported scope, such as large-team management, executive ownership, or specialized expertise the user clearly lacks, lower the match or exclude it using that concrete reason.

## Match classes

### Strong match

Clear fit across the important dimensions with no major unresolved conflict. Exact title equality is not required.

### Possible match

Plausible fit, but one or more material details are missing, ambiguous, or represent a reasonable stretch that the user may still want to consider.

### Weak match

Not a hard exclusion, but meaningfully outside the user's target or evidence. Normally omitted from the main shortlist.

## Explanation standard

Every Strong or Possible match should have a short evidence-based reason.

Prefer explanations such as:

- `Strong overlap with your B2C product design and funnel optimization experience; Lead title appears IC-focused.`
- `Staff title is above your usual target, but the required scope closely matches your demonstrated ownership and cross-functional leadership.`

Avoid generic praise such as `great fit` without pointing to role scope, domain, skills, career evidence, or constraints.

## Invalid or unreadable profile

If the private profile cannot be read, its YAML is materially malformed, or the content is too incomplete to determine the user's target:

- do not use ChatGPT Memory or old chats as a substitute;
- do not assign Strong, Possible, or Weak fit;
- report `PROFILE_UNAVAILABLE` or `PROFILE_INVALID` in Diagnostics;
- continue only with source-health and non-profile-dependent parsing checks when useful.

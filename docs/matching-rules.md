# Matching rules

## Decision order

1. Read and validate the private career profile.
2. Parse the posting.
3. Apply hard exclusions.
4. Deduplicate within the current run.
5. Compare with history when Tracker completeness is verified.
6. Evaluate soft fit.
7. Extract/validate the best usable application link.
8. Produce shortlist and TSV rows.

## Hard exclusions

Use only explicit profile rules or unambiguous posting facts.

Examples:
- title family listed in `excluded_titles`
- internship when internships are disallowed
- location outside an allowed region when location is a hard rule
- required sponsorship mismatch when configured as hard
- excluded non-digital domain
- a user-defined blocking keyword
- a required skill listed as a hard blocker when the posting clearly requires it

Missing information is not automatically a hard exclusion.

## Soft fit dimensions

### Title and seniority
Does the role correspond to a primary or adjacent target title and acceptable level?

### Domain
Is the product/industry close to the user's preferred experience or target direction?

### Skills
Does the posting emphasize strengths present in the private career profile and its experience evidence?

### Location/work model
Does the stated location and remote/hybrid/on-site model match preference?

### Employment/compensation
Is the employment type acceptable, and does known compensation meet the user's rule?

## Match classes

### Strong match
Clear fit across the important dimensions with no major unresolved conflict.

### Possible match
Plausible fit, but one or more material details are missing, ambiguous, or slightly outside preference.

### Weak match
Not a hard exclusion, but meaningfully outside the user's target. Normally omitted from the main shortlist.

## Explanation standard

Every Strong/Possible match should have a short evidence-based reason. Avoid generic praise such as `great fit` without pointing to title, domain, skills, career evidence, or constraints.

## Invalid or unreadable profile

If the private profile cannot be read, its YAML is materially malformed, or the content is too incomplete to determine the user's target:

- do not use ChatGPT Memory or old chats as a substitute;
- do not assign Strong/Possible/Weak fit;
- report `PROFILE_UNAVAILABLE` or `PROFILE_INVALID` in Diagnostics;
- continue only with source-health and non-profile-dependent parsing checks when useful.

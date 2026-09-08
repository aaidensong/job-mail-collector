# Profile questionnaire

The bootstrap prompt collects enough information to create a private career profile that can judge job fit without requiring the user to paste a resume on every scheduled run.

The result is written into the separate private profile document using `profiles/profile.template.md`.

## Required

### Target role
- primary titles
- adjacent titles
- target seniority
- excluded titles
- people-management preference

### Career evidence
- current/recent role family
- approximate years of relevant experience when useful
- most relevant products, domains, and responsibilities
- strongest skills
- selected outcomes or evidence that materially improve matching
- specialty areas or portfolio focus

The workflow should not require a complete chronological resume if a shorter factual career profile is enough.

### Domain and skills
- preferred product/domain/industry areas
- excluded domains
- strongest relevant skills and experience
- hard skill blockers, if any

### Geography and work model
- target locations
- remote/hybrid/on-site acceptance
- relocation or commute constraints when material

### Employment constraints
- acceptable employment types
- work authorization
- sponsorship handling rule
- minimum compensation if the user wants compensation filtering

### Exclusions and warnings
- hard-exclude keywords
- warning-only keywords
- company types to prefer/avoid

### Optional matching context
- languages relevant to work
- resume version labels
- industries the user is intentionally trying to enter
- industries the user wants to leave
- accessibility/security/domain expertise
- title nuances that need natural-language explanation

## Operational setup questions

These values go to the Sheet Config/Sources tabs, not the private career profile:

### Schedule
- daily run time
- time zone
- scan window

### Sources
- confirmed job-alert senders or domains
- which sources are digests

### Optional modules
- missing application detection
- response detection
- no-response detection and threshold
- rejection-stage estimation
- ATS estimation

## Rule severity

Career-related fields should distinguish between:

- `hard`: can exclude a job
- `soft`: changes match quality but does not exclude
- `info`: context only

The Markdown template represents this through dedicated fields such as `excluded_titles`, `hard_skill_blockers`, `hard_exclude_keywords`, and narrative interpretation notes rather than a flat spreadsheet RuleType column.

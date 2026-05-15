---
name: format
description: Second agent in Indian family-law drafting pipeline. Loads the case-type-specific skill template + the user's family-config.md from the case folder, maps facts from case-facts.md into format placeholders, and substitutes every personal-law-specific and State-specific value (statutory opening, cause-title court designation, applicable-section citations, Family-Court Rules reference). Outputs format-shell.md ready for the Drafter — already rendered in the appropriate personal-law + State idiom.
allowed-tools: Read, Bash, Glob
---

# Format Agent (family-config-aware)

Second in the 6-agent family-law drafting pipeline. Reference: `${CLAUDE_PLUGIN_ROOT}/skills/_family_pleading_base/SKILL.md`.

## Job

Pre-substitute every personal-law-specific and State-specific value into the case-type skill template, so the Drafter focuses entirely on the narrative.

## Inputs

- `<case-folder>/case-facts.md` (from Reader)
- `<case-folder>/family-config.md` (user-supplied)
- `${CLAUDE_PLUGIN_ROOT}/skills/<case-type>-draft/SKILL.md`
- `${CLAUDE_PLUGIN_ROOT}/skills/<case-type>-draft/format-from-user.md`
- `${CLAUDE_PLUGIN_ROOT}/skills/_family_pleading_base/SKILL.md`

## Outputs

- `<case-folder>/format-shell.md` — case-type template with all placeholders filled:
  - `{{family_config.court_designation}}` — e.g. *IN THE COURT OF THE FAMILY JUDGE AT NAGPUR* (Maharashtra Family Court) / *IN THE COURT OF THE DISTRICT JUDGE AT [PLACE]* (where Family Courts are not established under FCA 1984) / *IN THE COURT OF THE PRINCIPAL JUDGE, FAMILY COURT AT [PLACE]*
  - `{{family_config.applicable_personal_law}}` — Hindu Marriage Act 1955 / Special Marriage Act 1954 / Indian Divorce Act 1869 / Parsi Marriage and Divorce Act 1936 / Muslim Personal Law / Section 125 BNSS / etc.
  - `{{family_config.state}}` — State name (drives State-specific Family-Court Rules + State-specific procedural overlays)
  - `{{family_config.territorial_jurisdiction}}` — last cohabitation place / petitioner's residence / marriage registration place (per Section 19 HMA / Section 31 SMA / etc.)
  - `{{case_type.statutory_opening}}` — case-type and personal-law-specific opening (e.g. *PETITION UNDER SECTION 13(1)(ia) OF THE HINDU MARRIAGE ACT, 1955 FOR DISSOLUTION OF MARRIAGE BY DECREE OF DIVORCE ON THE GROUND OF CRUELTY*)
  - `{{case_type.relief_clause}}` — case-type primary relief
  - `{{case_type.verification_form}}` — Family-Court verification form (varies between FCA-1984 States and non-FCA States)

The Drafter agent reads `format-shell.md` and writes the narrative without having to redo any of the personal-law / State-specific substitution.

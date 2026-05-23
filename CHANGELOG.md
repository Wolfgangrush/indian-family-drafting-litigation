# Changelog

All notable changes to the `indian-family-drafting` plugin are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [0.2.1-alpha] — 2026-05-24

### Filing-grade render-defect repair + pipeline-optionality

The v0.1.0 render path produced filing-grade Markdown but the pandoc → `.docx` conversion failed Bombay HC / equivalent Registry expectations on multiple counts (title not bold, section headers left-aligned, Index table column-headers wrapping vertically, party block leaking onto cover pages, ~6,200-word bloat). This release repairs the render path, calibrated against an actual filed Bombay HC Nagpur Second Appeal pleading the author supplied as the filing-grade reference. Inherits the v0.2.1 fixes from `indian-hc-drafting-litigation`.

### Added

- **Pre-customised `reference.docx`** in the plugin's base-skill folder with locked Word styles (TNR 14pt body, 1.5 line spacing, 4cm left / 2.5cm right-top-bottom margins, Heading 1 bold centered, Heading 2 bold + UNDERLINED + centered + letter-spacing for the spaced `F A C T S` effect, Heading 3 bold + UNDERLINED + centered for unspaced section headers, Heading 4 bold + UNDERLINED + left for `MOST RESPECTFULLY SHEWETH:` style anchors, fixed table layout).
- **`build_reference_docx.py`** — reproducible python-docx build script for the shipped reference.docx.
- **`fix_docx_tables.py`** — post-pandoc Python script that forces column widths on every table (5-col 8/8/60/14/10; 4-col 10/10/65/15; 3-col 10/75/15; 2-col 18/82). Locks first-row bold + centered + vertically-centered cells. Drafter runs this as the final post-pandoc step.
- **MARKDOWN HEADING DISCIPLINE** in the Drafter prompt + base SKILL.md (Heading 1 / Heading 2 / Heading 3 / Heading 4 mapping for court header / spaced section headers / unspaced section headers / left-anchored headings).
- **VERBOSITY DISCIPLINE** with per-case-type word-count targets and hard ceilings.
- **PIPELINE-OPTIONALITY** — Verifier / Refiner / Overseer now OPTIONAL QC layers. Default exit point is after Stage 3 (Drafter).
- **COVER-PAGE DISCIPLINE** — INDEX / SYNOPSIS / LIST OF ANNEXURES each begin on `\newpage` with short cause-title only.
- **Bold-number paragraph convention** — Facts and Grounds paragraphs use `**1.** **2.** **3.**`.
- **Inline-bold highlighting convention** for property descriptors / annexure markers / key terms within Facts narrative.

### Changed

- **Drafter pandoc command** is now TWO steps (pandoc → .docx, then `fix_docx_tables.py`). Step 2 is non-negotiable; skipping it reproduces the v0.2.0 stacking-column defect.

### Cost / token-budget note

Running the full 6-agent pipeline burns approximately 600K tokens per draft, which can exhaust an advocate's Claude session limit. v0.2.1 makes Stages 4–6 OPTIONAL so a baseline Reader → Format → Drafter run (~280K tokens) is sufficient for routine pleadings. The optional QC stages remain available for high-stakes matters.

---

## [0.1.0-alpha] — 2026-05-15 (initial release)

### Added

- **Plugin scaffolding** — `.claude-plugin/plugin.json` manifest, MIT `LICENSE`, `NOTICE.md` provenance and privilege statement, `.gitignore`, this `CHANGELOG.md`, comprehensive `README.md`.
- **Six-agent drafting pipeline** — Reader → Format → Drafter → Verifier → Refiner → Overseer. Each agent is a markdown file under `agents/<name>/<name>.md` with YAML frontmatter declaring `name`, `description`, and `allowed-tools`.
- **Shared infrastructure skills:**
  - `_drafting_common` — anti-pollution rules, encoding standards, language conventions, AI-style-marker blacklist, family-court-specific privacy firewall (real names, minor identifiers, intimate-partner details substituted with placeholders before any AI processing).
  - `_family_pleading_base` — universal Family-Court / matrimonial-jurisdiction pleading skeleton (cause title, parties, statutory opening, prayer template, verification clause, supporting affidavit, schedule of property where applicable, list of documents) — reads State + personal-law overlay from the user's `family-config.md`.
- **Eight case-type skill stubs** — divorce-petition · judicial-separation · restitution-conjugal-rights · nullity-petition · maintenance-application · custody-petition · dv-act-application · adoption-petition.
- **Personal-law coverage stub** — Hindu Marriage Act 1955 · Special Marriage Act 1954 · Indian Divorce Act 1869 · Parsi Marriage and Divorce Act 1936 · Muslim Personal Law (Shariat) Application Act 1937 + Dissolution of Muslim Marriages Act 1939 · Family Courts Act 1984 · Hindu Adoptions and Maintenance Act 1956 · Hindu Minority and Guardianship Act 1956 · Guardians and Wards Act 1890 · Protection of Women from Domestic Violence Act 2005 · Section 125 BNSS (corresponding to Section 125 CrPC).
- **State / forum awareness** — the user supplies `family-config.md` declaring the State, the Family Court territorial jurisdiction, and the applicable personal law for the parties; the plugin renders the pleading in the appropriate idiom.

### Notes on this release

This is a **v0.1.0-alpha scaffold release**. The structural skeleton, agent pipeline, base skills, and case-type skill frames are in place; deep per-skill encoding (case-specific Grounds frames, ground-by-ground citation maps, State-specific Family Court Rules calibration) will land in v0.1.0 and onward as community contribution arrives and the author's own validation deepens.

### Released under

MIT License. Authored by Rushikesh R. Mahajan, Advocate, publishing under the Wolfgang Rush open-source brand for legal-tech infrastructure.

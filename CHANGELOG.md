# Changelog

All notable changes to the `indian-family-drafting` plugin are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/).

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

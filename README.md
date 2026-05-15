# indian-family-drafting

> **Open-source Claude-compatible plugin for drafting pleadings before Indian Family Courts and the matrimonial-jurisdiction tier of District Courts.**
>
> Six-agent drafting pipeline · eight family-law case-type skills · personal-law-aware across HMA / SMA / IDA / Parsi / Muslim Personal Law / DV Act / G&W Act / HAMA / JJ Act.
>
> Released under MIT. Open infrastructure for the legal community. No commercial engagement offered through this repository — see Disclaimer below.

---

## Table of contents

1. [What this plugin does](#what-this-plugin-does)
2. [Personal-law coverage](#personal-law-coverage)
3. [Case-type skills (full inventory with statutory authority)](#case-type-skills-full-inventory)
4. [The 6-agent drafting pipeline (what each agent does)](#the-6-agent-drafting-pipeline)
5. [Installation](#installation) — Claude Code CLI **and** Claude Desktop application
6. [Your first family-law pleading — step-by-step walkthrough](#your-first-family-law-pleading)
7. [The `family-config.md` file — how personal-law and forum customisation works](#the-family-configmd-file)
8. [Privacy firewall — extra discipline for family-law content](#privacy-firewall--extra-discipline-for-family-law-content)
9. [Why MIT License](#why-mit-license)
10. [Sibling plugins](#sibling-plugins)
11. [Why this exists](#why-this-exists)
12. [Roadmap](#roadmap)
13. [Contributing](#contributing)
14. [Contact](#contact)
15. [Author and brand](#author-and-brand)
16. [Provenance and privilege statement](#provenance-and-privilege-statement)
17. [Disclaimer and Bar Council of India Rule 36 compliance](#disclaimer-and-bar-council-of-india-rule-36-compliance)
18. [License](#license)

---

## What this plugin does

This plugin lets an Indian advocate, sitting inside any Claude-compatible AI runtime (Claude Code CLI or the Claude Desktop application), point to a case folder on disk and obtain a complete family-law pleading in `.docx` form — Cause Title, Marriage Particulars, Domestic Chronology, Grievance Narrative, Grounds, Prayer, Reconciliation / ADR averment, Verification, Supporting Affidavit, List of Documents, and the accompanying applications (interim maintenance, interim custody, ex parte interim DV-Act orders, mediation reference, etc.) — formatted in the **personal-law-appropriate idiom** sourced from a `family-config.md` file the user places in the case folder.

The pipeline is six agents running in sequence:

1. **Reader** — extracts facts from the case folder with a per-document audit log, identifies annexure candidates, and **applies the family-law privacy firewall** (every party name, every minor's name, every intimate-partner detail substituted with a structural placeholder before any downstream AI processing — the mapping back to real names is kept locally on the user's machine in the header of `case-facts.md`).
2. **Format** — loads the case-type skill template (e.g. `divorce-petition-draft`, `dv-act-application-draft`), reads the user's `family-config.md`, and substitutes the personal-law-specific statutory opening, court designation, applicable-section citations, and verification form into a `format-shell.md` ready for the Drafter.
3. **Drafter** — writes the actual narrative — marriage particulars, domestic chronology, grievance events, Grounds per the case-type structure, Prayer, Reconciliation / ADR averment, Verification, Supporting Affidavit, List of Documents, and any accompanying application that the facts require. Outputs `draft-v1.docx` and `draft-v1.md`.
4. **Verifier** — anti-hallucination firewall. Compares every fact in the draft against the Reader's extracted `case-facts.md`. Flags fabricated dates (marriage / separation / DV-event dates that do not trace), mis-cited sections, orphan annexure markers, unsupported assertions, **hallucinated case citations** (especially headline family-law cases — *Shilpa Sailesh v. Varun Sreenivasan*, *Rajnesh v. Neha*, *Indra Sarma v. V.K.V. Sarma*, *Satish Chander Ahuja v. Sneha Ahuja*, *Gaurav Nagpal v. Sumedha Nagpal*, *Joseph Shine v. Union of India* — whose accompanying year / volume / SCC-page reference is the most-frequently-mis-remembered detail), the *Rajnesh v. Neha* maintenance computation cross-foot, the **personal-law applicability check**, and **Section 7 Family Courts Act jurisdictional sanity**.
5. **Refiner** — applies Verifier flags, polishes language to Family-Court formal register, enforces State-specific Family-Court Rules formatting per the `family-config.md`, strips AI-style markers, and re-substitutes real party names from the privacy-firewall mapping into the final `.docx` (strictly on the user's local machine — the underlying AI never holds the real names).
6. **Overseer** — reads the polished draft with an opposing-counsel lens. Flags weak prayers, contradictory facts, attackable defects in the Grounds, gaps in the chronology, jurisdictional vulnerabilities, maintenance computation weak spots, custody-position vulnerabilities, Section 9 FCA reconciliation-clause omission, DV-Act element check (`domestic relationship` / `shared household` under Sections 2(f) / 2(s) DV Act), and missing limbs of argument.

The output is what an advocate would file before a Family Court / District Court exercising matrimonial jurisdiction. **Not a template. Not a checklist. A pleading.**

---

## Personal-law coverage

India is a multi-personal-law jurisdiction. Family-law matters are governed by the personal law of the parties (Hindu / Muslim / Christian / Parsi / Special Marriage), supplemented by secular statutes (Domestic Violence Act, Section 125 BNSS, Family Courts Act). This plugin supports the full personal-law spectrum:

| Personal law | Statute | Plugin coverage |
|---|---|---|
| **Hindu** (incl. Buddhist, Jain, Sikh) | Hindu Marriage Act 1955 (HMA) | Full — divorce / JS / RCR / nullity / maintenance / custody |
| **Hindu** (adoption, maintenance) | Hindu Adoptions and Maintenance Act 1956 (HAMA) | Full — adoption, Section 18 maintenance |
| **Hindu** (guardianship of minor) | Hindu Minority and Guardianship Act 1956 (HMG Act) | Full — read with the Guardians and Wards Act 1890 |
| **Secular (registered)** | Special Marriage Act 1954 (SMA) | Full — divorce / JS / RCR / nullity / maintenance / custody |
| **Christian** | Indian Divorce Act 1869 (IDA, as amended 2001) + Indian Christian Marriage Act 1872 | Full — divorce / JS / nullity / maintenance |
| **Parsi** | Parsi Marriage and Divorce Act 1936 | Coverage scaffolded — community contribution welcomed for Parsi Chief Matrimonial Court conventions |
| **Muslim (wife's remedy)** | Dissolution of Muslim Marriages Act 1939 | Full — Section 2 grounds for dissolution by the wife |
| **Muslim (husband's talaq)** | Muslim Personal Law (Shariat) Application Act 1937 + Muslim Women (Protection of Rights on Marriage) Act 2019 (governing triple talaq) | Coverage scaffolded |
| **Domestic violence (all women)** | Protection of Women from Domestic Violence Act 2005 | Full — Sections 12, 18–22 reliefs, with post-*Satish Chander Ahuja* shared-household understanding |
| **Maintenance — secular** | Section 125 BNSS 2023 (formerly Section 125 CrPC) | Full — Magistrate / Family Court forum |
| **Senior citizens' maintenance** | Maintenance and Welfare of Parents and Senior Citizens Act 2007 | Coverage scaffolded |
| **Adoption — secular / orphan** | Juvenile Justice (Care and Protection of Children) Act 2015 + Adoption Regulations 2022 + CARA | Full — JJ Act in-country and inter-country adoption |
| **Custody and guardianship — secular** | Guardians and Wards Act 1890 | Full |

The applicable personal law for a given case is declared in the user's `family-config.md`. The Verifier checks the declared personal law against the parties' religion as recorded in `case-facts.md` — a mismatch (e.g. HMA divorce filed where one party is a Muslim) is flagged at Verifier stage and corrected at Refiner.

---

## Case-type skills (full inventory)

The plugin ships **eight case-type skills**, each grounded in the statutory authority below.

### 1. `divorce-petition-draft`

**Statutory authority:** HMA 1955 Section 13 / Section 13B · SMA 1954 Section 27 / Section 28 · IDA 1869 Section 10 / Section 10A · Parsi Marriage and Divorce Act 1936 Section 32 · Dissolution of Muslim Marriages Act 1939 Section 2 (for the Muslim wife) · Muslim Women (Protection of Rights on Marriage) Act 2019 (for triple-talaq cases). **Use case:** dissolution of marriage. **Facts asked for:** parties + religion, marriage particulars, domestic chronology, the ground invoked (cruelty / desertion / adultery / mutual consent / etc.), prior proceedings, financial-disclosure data per *Rajnesh v. Neha*, property and custody position. **Output:** complete divorce petition + Supporting Affidavit + List of Documents + interim-maintenance and interim-custody applications.

### 2. `judicial-separation-draft`

**Statutory authority:** HMA Section 10 / SMA Section 23 / IDA Section 22. **Use case:** judicial separation as a precursor to divorce on the ground of non-resumption-of-cohabitation, or where the petitioner prefers separation over dissolution. **Facts asked for:** same as divorce, on the same grounds.

### 3. `restitution-conjugal-rights-draft`

**Statutory authority:** HMA Section 9 / SMA Section 22 / IDA Section 32. **Use case:** decree directing the respondent to resume cohabitation; commonly filed as a procedural step preceding divorce-after-one-year under HMA Section 13(1A)(ii).

### 4. `nullity-petition-draft`

**Statutory authority:** HMA Section 11 (void) / Section 12 (voidable) · SMA Section 24 / Section 25 · IDA Section 18 / Section 19. **Use case:** declaration that the marriage is void ab initio (bigamy, prohibited consanguinity, ceremony not performed) or voidable (impotence, fraud, force, pregnancy by third party unknown to husband, mental incapacity).

### 5. `maintenance-application-draft`

**Statutory authority:** Section 125 BNSS 2023 · Section 24 HMA · Section 25 HMA · Section 18 HAMA · Section 20 DV Act · Section 36 SMA · Section 37 SMA · Section 36 IDA · Section 23 Maintenance and Welfare of Parents and Senior Citizens Act 2007. **Use case:** the full menu of maintenance remedies — interim, permanent, secular, personal-law-based, DV-Act-based. **Mandatory:** the *Rajnesh v. Neha (2021) 2 SCC 324* affidavit-of-disclosure (income / assets / liabilities / lifestyle / dependants) for both parties.

### 6. `custody-petition-draft`

**Statutory authority:** Guardians and Wards Act 1890 · Hindu Minority and Guardianship Act 1956 (where the child is Hindu). **Use case:** appointment of guardian + custody + visitation + vacation custody + education / medical decision-making authority. **Welfare-of-the-child principle** (*Gaurav Nagpal v. Sumedha Nagpal*) is the paramount consideration. **International parental child-removal:** *Yashita Sahu v. State of Rajasthan* + *Surya Vadanan v. State of Tamil Nadu* parens-patriae framework.

### 7. `dv-act-application-draft`

**Statutory authority:** Protection of Women from Domestic Violence Act 2005 (Sections 12, 18, 19, 20, 21, 22, 23). **Use case:** Protection Order + Residence Order + Monetary Relief + Custody Order + Compensation + ex parte interim orders. **Mandatory pleadings:** Section 2(f) "domestic relationship" + Section 2(s) "shared household" (per post-*Satish Chander Ahuja v. Sneha Ahuja (2021) 1 SCC 414* understanding, which overruled *S.R. Batra*) + Section 3 acts of domestic violence (physical / sexual / verbal / emotional / economic).

### 8. `adoption-petition-draft`

**Statutory authority:** Hindu Adoptions and Maintenance Act 1956 (HAMA, for Hindu adoptive parents + Hindu child) · Juvenile Justice (Care and Protection of Children) Act 2015 (secular, CARA-mediated) · Adoption Regulations 2022 · Hague Convention on Inter-Country Adoption 1993 (India signatory). **Use case:** declaration of validity of adoption (HAMA route — capacity, ceremony, no-consideration discipline) or court-mediated adoption (JJ Act route — CARA process + Child Welfare Committee Section 38 declaration).

### Shared infrastructure skills

- **`_drafting_common`** — anti-pollution rules, family-law-specific privacy firewall, AI-style-marker blacklist, citation discipline, personal-law applicability discipline, Family Courts Act Section 9 reconciliation discipline.
- **`_family_pleading_base`** — universal Family-Court / matrimonial-jurisdiction pleading skeleton (Cause Title, Parties, Statutory Opening, Marriage Particulars, Domestic Chronology, Grievance Narrative, Jurisdiction, Prior Proceedings, Grounds, Reconciliation/ADR, Prayer, Verification, Supporting Affidavit, List of Documents, Advocate's Signature, Accompanying Applications) — reads State + personal-law overlay from the user's `family-config.md`.

---

## The 6-agent drafting pipeline

The plugin is built on the **Anthropic Agent SDK** convention — six markdown agent files (`agents/<name>/<name>.md`) with YAML frontmatter declaring `name`, `description`, and `allowed-tools`. Each agent is invoked in sequence on a case folder.

| Agent | What it reads | What it writes | Key family-law specialisation |
|---|---|---|---|
| **`reader`** | Every file in the case folder + the case-type skill's `case-facts-questions.md` | `case-facts.md` with per-document audit log + `annexure-candidates.md` + `missing-laws.md` (halts pipeline if any required statute is missing) | Privacy firewall — substitutes every party name, minor's name, intimate-partner detail with placeholders before downstream AI processing; mapping stored locally only |
| **`format`** | `case-facts.md` + `family-config.md` + case-type SKILL.md + `_family_pleading_base` | `format-shell.md` — case-type template with personal-law statutory opening, court designation, applicable-section citations, verification form pre-substituted | Personal-law-aware substitution (HMA / SMA / IDA / Parsi / Muslim PL / FCA-1984 / DV Act) |
| **`drafter`** | `case-facts.md` + `format-shell.md` + case-type SKILL.md + `_family_pleading_base` + law PDFs | `draft-v1.md` + `draft-v1.docx` — Main Pleading + Supporting Affidavit + List of Documents + Accompanying Applications | Family-Court formal register, *Rajnesh v. Neha* maintenance framework, Section 9 FCA reconciliation averment, *Satish Chander Ahuja* shared-household understanding |
| **`verifier`** | `draft-v1.md` + `case-facts.md` + `family-config.md` + law PDFs | `verification-report.md` flagging fabrications, mis-citations, orphan markers, unsupported assertions, hallucinated case citations, *Rajnesh v. Neha* computation cross-foot, personal-law applicability check, Section 7 FCA jurisdictional sanity | Citation discipline (the headline family-law cases — *Shilpa Sailesh*, *Indra Sarma*, *Satish Chander Ahuja*, *Rajnesh v. Neha*, *Gaurav Nagpal*, *Joseph Shine* — are the most-mis-remembered by AI) |
| **`refiner`** | `draft-v1.md` + `verification-report.md` + `family-config.md` + `case-facts.md` | `draft-v2.md` + `draft-v2.docx` | Family-Court Rules formatting per State + privacy-firewall reversal (real names re-substituted from local mapping into final `.docx`) |
| **`overseer`** | `draft-v2.md` + `case-facts.md` + `family-config.md` | `opposing-notes.md` + `final-draft.docx` | Opposing-counsel critique with family-law-specific attack vectors: chronology gaps, jurisdiction vulnerabilities, maintenance weak spots, custody attackable points, DV-Act element omissions, Section 9 FCA reconciliation omission |

---

## Installation

This is a Claude-compatible plugin in the Anthropic plugin format. It runs inside any Claude-compatible runtime — the **Claude Code CLI** or the **Claude Desktop application** — and can also run inside a custom Anthropic-API-based runtime that respects the `SKILL.md` frontmatter convention.

### Option A — Claude Code (terminal CLI)

```bash
mkdir -p ~/.claude/plugins
cd ~/.claude/plugins
git clone https://github.com/Wolfgangrush/indian-family-drafting-litigation.git indian-family-drafting
claude plugin list
```

### Option B — Claude Desktop application

| OS | Plugin folder path |
|---|---|
| **macOS** | `~/Library/Application Support/Claude/plugins/` |
| **Windows** | `%APPDATA%\Claude\plugins\` (typically `C:\Users\<you>\AppData\Roaming\Claude\plugins\`) |
| **Linux** | `~/.config/Claude/plugins/` |

Clone the plugin into that folder, then restart the Claude Desktop application. The plugin is auto-discovered on the next session start.

### Verifying the install

In a Claude session, type:

- *"draft divorce"* — triggers `divorce-petition-draft`
- *"draft DV application"* — triggers `dv-act-application-draft`
- *"draft maintenance"* — triggers `maintenance-application-draft`
- *"draft custody"* — triggers `custody-petition-draft`
- *"draft adoption"* — triggers `adoption-petition-draft`
- `/divorce-petition-draft` — explicit slash-invocation

---

## Your first family-law pleading

Suppose you wish to draft a **divorce petition under HMA Section 13(1)(ia)** (cruelty) before the **Family Court at Nagpur**.

### Step 1 — create a case folder

```
~/Desktop/cases/
└── hma-divorce-CASE/
    ├── family-config.md          ← declares HMA + Maharashtra + Family Court Nagpur
    ├── facts/
    │   ├── marriage-cert.pdf
    │   ├── medical-records-cruelty.pdf
    │   ├── police-complaint.pdf
    │   ├── messages.pdf
    │   ├── salary-slip.pdf
    │   └── itr.pdf
    ├── laws/
    │   ├── hindu-marriage-act-1955.pdf
    │   ├── family-courts-act-1984.pdf
    │   └── rajnesh-v-neha-2021.pdf
    └── notes.md
```

### Step 2 — write the `family-config.md`

```yaml
state: "Maharashtra"
court_designation: "IN THE COURT OF THE PRINCIPAL JUDGE, FAMILY COURT AT NAGPUR"
applicable_personal_law: "Hindu Marriage Act 1955"
territorial_jurisdiction_basis: "Section 19(i) HMA — marriage was solemnised within the territorial limits of this Court / Section 19(ii) HMA — respondent resides within these limits"
family_court_rules_reference: "Maharashtra Family Courts Rules 1988"
verification_clause_form: "as prescribed under the Maharashtra Family Courts Rules 1988"
case_type: "divorce"
sub_ground: "cruelty"
```

### Step 3 — launch Claude inside the case folder

```bash
cd ~/Desktop/cases/hma-divorce-CASE/
claude
```

### Step 4 — invoke the skill

```
draft divorce
```

The Reader will walk every file, write `case-facts.md` with marriage chronology, identify EXHIBIT slots, apply the privacy firewall (party names replaced with `[Petitioner]` / `[Respondent]` / `[Minor-Child-1]` etc.), and halt if any required law PDF is missing.

Verify `case-facts.md`. Save.

### Step 5 — continue the pipeline

**Format → Drafter → Verifier → Refiner → Overseer** run in sequence. At the end:

- `draft-v1.docx` — initial draft
- `verification-report.md` — Verifier flags
- `draft-v2.docx` — Refiner output (real party names re-substituted here from local mapping)
- `opposing-notes.md` — Overseer critique
- `final-draft.docx` — for your review

### Step 6 — review, sign, file

Open `final-draft.docx`. Verify every fact. Verify every citation. Verify the Section 9 FCA reconciliation averment. Verify the *Rajnesh v. Neha* affidavit-of-disclosure is attached. Sign. File before the Family Court Nagpur Registry along with the prescribed court fee + the *Rajnesh v. Neha* disclosure affidavits of both parties.

**You are responsible for the pleading. The plugin is responsible for the first draft.**

---

## The `family-config.md` file

The `family-config.md` is how the plugin knows *which personal law* and *which forum* apply to your case. A typical config has fields like:

```yaml
state: "Maharashtra"
court_designation: "IN THE COURT OF THE PRINCIPAL JUDGE, FAMILY COURT AT NAGPUR"
applicable_personal_law: "Hindu Marriage Act 1955"       # HMA / SMA / IDA / Parsi / Muslim_DMMA / etc.
territorial_jurisdiction_basis: "Section 19 HMA — last cohabitation place / petitioner residence / marriage place"
family_court_rules_reference: "Maharashtra Family Courts Rules 1988"
verification_clause_form: "Maharashtra Family Court Rules form"
supporting_affidavit_form: "Maharashtra Family Court Rules Form II"
case_type: "divorce"                  # divorce / judicial-separation / RCR / nullity / maintenance / custody / DV / adoption
sub_ground: "cruelty"                 # case-type-specific ground
forum_class: "FCA_1984_Family_Court"  # vs "District_Court_Matrimonial" (in States without Family Courts)
mediation_clause: "mandatory"          # per Section 9 FCA 1984
```

The Format agent reads this file and pre-substitutes every personal-law-specific and forum-specific value into the case-type template. If your State's Family Court Rules are revised, you edit `family-config.md` — never the plugin source.

---

## Privacy firewall — extra discipline for family-law content

Family-law cases routinely contain the **most sensitive personal data** the plugin will encounter — minor children's names and DOBs, intimate-partner conduct allegations, mental-health disclosures, financial disclosures of both spouses, religious affiliation, and (in DV Act matters) shared-household particulars including the conduct of the respondent's relatives. The plugin's privacy discipline is therefore **stricter** for family-law than for any sibling plugin.

The discipline (declared in `_drafting_common`):

1. **Reader** substitutes every party name, every minor's name, every intimate-partner detail with structural placeholders before any downstream AI processing.
2. The placeholder → real-name mapping is stored in the header of `case-facts.md` on the user's local machine **only**.
3. **Format / Drafter / Verifier / Overseer** operate only on the placeholder-substituted content. The underlying AI runtime never holds the real names.
4. **Refiner** is the only agent that re-substitutes real names into the final `.docx`, strictly on the user's local machine, by reading the local-only mapping.
5. The `.gitignore` explicitly excludes `case-facts.md` so it cannot be committed accidentally.

**This is structural, not optional.** The plugin will not produce a final draft with real party names visible to the AI runtime; the real names appear in the final `.docx` only via the Refiner's local re-substitution step.

---

## Why MIT License

The plugin is released under the **MIT License**. This was a deliberate choice. The alternatives — and why they were rejected — are below.

### MIT vs the alternatives

| License | Suitable for this plugin? | Reasoning |
|---|---|---|
| **MIT** ✅ chosen | Yes | Permissive · 3-line attribution requirement · zero copyleft · zero patent-grant complexity · compatible with the Anthropic Plugin Marketplace TOS · compatible with adoption by Family-Court advocates, matrimonial chambers, legal-aid clinics handling DV-Act matters, and CARA-recognised adoption agencies · allows a future paid commercial layer to ship under a separate corporate entity without dual-license complexity. |
| **Apache 2.0** | Close second | Adds an explicit patent grant — but Indian procedural drafting content is non-patentable under Section 3(k) of the Patents Act 1970. The patent grant is dead weight; NOTICE-file ceremony adds friction. |
| **GPL-3.0** | ❌ Disqualifying | Copyleft would propagate to the advocate's case folder. The pleading — generated by the plugin from the advocate's case facts — could be argued to be a "derivative work", forcing the pleading itself to be GPL-licensed. **No advocate can ship privileged client material** (and family-law cases contain among the most sensitive privileged material in any legal practice) **under any open-source licence.** Hard structural blocker. |
| **AGPL-3.0** | ❌ Disqualifying | Same problem as GPL-3, plus the network-use clause triggers if anyone integrates the plugin into a SaaS legal-tech product. Blocks all commercial integration. |
| **LGPL-3.0** | ❌ Awkward | Designed for shared libraries, not for plugins that produce derivative documents. |
| **BSD-3-Clause / BSD-2-Clause** | Functionally equivalent to MIT | No practical advantage. MIT is more widely understood. |
| **Unlicense / CC0** | ❌ Forfeits authorship | Drops the copyright assertion. The author loses moral rights under Section 57 Copyright Act 1957. |
| **Creative Commons (any variant)** | ❌ Wrong instrument | CC licences are designed for creative content, not software. |

### One-paragraph rationale

The plugin is released under MIT because Family-Court advocates, matrimonial chambers, legal-aid clinics, and the broader community of advocates handling domestic-violence, maintenance, custody, and adoption matters across India must be able to clone, fork, adapt, and integrate this plugin alongside their privileged client material — which in family-law practice is the most sensitive privileged material an advocate handles — without the licence propagating into their case folders or attaching to the pleadings they file. Only MIT (and equivalently BSD and Apache 2.0) satisfies that constraint. MIT was preferred over Apache 2.0 because the patent-grant language Apache adds carries no practical benefit in the Indian context (procedural drafting content is unpatentable under Section 3(k) of the Patents Act 1970), and MIT's three-line clarity is friendlier to advocates and clinic volunteers who will read the LICENSE file before adopting the plugin.

---

## Sibling plugins

This plugin is part of the **Wolfgang Rush legal-tech family** of open-source India-legal-drafting plugins:

- **`indian-hc-drafting`** — bundled coverage across all 25 Indian High Courts (bench-config-aware) · [github.com/Wolfgangrush/indian-hc-drafting-litigation](https://github.com/Wolfgangrush/indian-hc-drafting-litigation)
- **`supreme-court-drafting`** — Supreme Court of India pleadings · [github.com/Wolfgangrush/supreme-court-drafting-litigation](https://github.com/Wolfgangrush/supreme-court-drafting-litigation)
- **`district-court-drafting`** — District Court civil + criminal pleadings, all-India · [github.com/Wolfgangrush/district-court-drafting-litigation](https://github.com/Wolfgangrush/district-court-drafting-litigation)
- **`indian-family-drafting`** — this plugin · [github.com/Wolfgangrush/indian-family-drafting-litigation](https://github.com/Wolfgangrush/indian-family-drafting-litigation)
- **`indian-contracts-drafting`** (forthcoming) — commercial contracts, conveyancing, personal estate
- **`indian-regulatory-drafting`** (forthcoming) — IT Act, Income Tax, Consumer Protection, MV Act regulatory notices and responses

Each plugin ships independently. Install only the ones you use.

---

## Why this exists

Family-law practice in India sits at the intersection of multiple personal-law regimes, multiple procedural statutes (Family Courts Act, CPC, BNSS, DV Act, JJ Act), and multiple forum architectures (Family Courts in some States, District Courts in others). Generic AI tools do not understand this matrix; they conflate HMA with SMA, mis-cite headline cases that every senior matrimonial lawyer knows by heart, miss the Section 9 Family Courts Act reconciliation clause, and produce drafts that read superficially correct but fail at the threshold scrutiny of a Family-Court Judge or a District Judge exercising matrimonial jurisdiction.

Family-law is also the practice area where **client confidentiality is most acute** — minor children, intimate-partner conduct, mental-health disclosures, domestic-violence allegations, religion-based grounds. The plugin's privacy firewall is structural, not optional: real names never leave the user's machine, and the underlying AI runtime never sees them.

This plugin opens that door. It is most-deeply-validated at the Family Court Nagpur (the author's primary practice court) for Hindu Marriage Act matters; other personal-law regimes and States are supported via `family-config.md`, with deepening per-State and per-personal-law validation arriving through community contribution.

---

## Roadmap

- [x] **v0.1.0-alpha** — six-agent pipeline + universal family pleading base + 8 case-type skill scaffolds (divorce / JS / RCR / nullity / maintenance / custody / DV / adoption) + family-law privacy firewall + personal-law applicability discipline + Section 9 FCA reconciliation discipline
- [ ] **v0.1.0** — full divorce / maintenance / DV / custody case-type coverage to chamber-grade across HMA + SMA + DV Act + Section 125 BNSS
- [ ] **v0.2.0** — IDA + Parsi + Muslim Personal Law deepening + Family-Court Rules calibration across Maharashtra + Karnataka + Tamil Nadu
- [ ] **v0.3.0** — adoption (HAMA + JJ Act) full coverage + senior-citizen maintenance under MWPSC Act 2007
- [ ] **v0.4.0** — international parental child-removal scenarios (*Yashita Sahu* + *Surya Vadanan* parens-patriae framework) + habeas corpus cross-reference into HC plugin
- [ ] **v1.0.0** — Stable release with community-validated coverage across all personal-law regimes and all State Family-Court Rules

---

## Contributing

Advocates with regular family-law practice in any State are invited to contribute:

- State-specific Family Court Rules references
- Personal-law-specific Grounds frames (especially Parsi Chief Matrimonial Court conventions and Muslim Personal Law calibrations)
- DV-Act Domestic Incident Report templates from your State's Protection Officer network
- *Rajnesh v. Neha* affidavit-of-disclosure templates calibrated to your State's Family Court format
- CARA / Adoption Regulations 2022 procedural updates
- Recent High Court / Supreme Court family-law judgments that change the law

Open a GitHub issue with your State + the contribution. Pull requests welcome.

This plugin is open source under MIT. No proprietary gating. No login. No telemetry. No tracking of who uses the plugin or what cases they file.

---

## Contact

All inquiries and feedback via **GitHub Issues** on the project repository.

This project does not have an email contact channel and **does not accept private legal-services inquiries through this repository**. No commercial engagement is offered through this plugin or its repository.

*(Future releases may introduce a commercial layer published under a separate corporate entity — at that point this section will be updated. v0.x.x is open-source-only infrastructure with no commercial channel.)*

---

## Author and brand

This plugin is authored by **Rushikesh R. Mahajan**, Advocate, enrolled with the Bar Council of Maharashtra and Goa, practising before the Bombay High Court (Nagpur Bench).

The plugin is published under the **Wolfgang Rush** open-source brand — the author's publishing handle for legal-technology infrastructure. All commits to this repository are signed under the Wolfgang Rush GitHub identity. The real-identity declaration appears here, and again in `NOTICE.md`, so that the Bar Council Rule 36 accountability mechanism (advocate-as-individual responsibility) is preserved transparently rather than displaced by the publishing handle.

---

## Provenance and privilege statement

See [`NOTICE.md`](./NOTICE.md) for the full provenance and privilege statement.

**In brief:** this plugin contains only public procedural knowledge — Family Court Rules conventions, personal-law statutes (HMA / SMA / IDA / Parsi / Muslim PL), secular statutes (DV Act / Section 125 BNSS / G&W Act / HAMA / HMG Act / FCA 1984 / JJ Act 2015 / MWPSC Act 2007), and generic placeholders. It does **not** contain any specific client matter, communication, document, or personal data.

---

## Disclaimer and Bar Council of India Rule 36 compliance

This plugin is **open-source infrastructure released free of cost** under the MIT licence.

**This plugin:**

- Does **not** constitute legal advice.
- Does **not** create an advocate-client relationship between the author and any user.
- Does **not** solicit professional work from any user, group, or audience.
- Does **not** advertise the professional services of the author or any advocate.
- Does **not** offer paid legal services, paid consultations, or commercial legal engagements through this repository or any associated channel.

**It is:**

- A drafting aid for use by **enrolled advocates** who retain full professional responsibility for every pleading produced.
- A reference implementation of open-source legal-tech infrastructure for Indian family-law practice.
- Released under the Bar Council of India Rules, Part VI, Chapter II, Section IV, **Rule 36**.

**Every advocate using this plugin is reminded:** the advocate retains full professional responsibility for the verification of facts, the accuracy of citations, the correctness of legal grounds, the propriety of the prayer, and the signature on every pleading filed. AI-generated drafting output is **starting material, not a finished pleading**.

---

## License

**MIT.** See [`LICENSE`](./LICENSE) for the full text.

Copyright (c) 2026 Wolfgang Rush. Authored by Rushikesh R. Mahajan, Advocate, publishing under the Wolfgang Rush open-source brand.

---
name: _drafting_common
description: Shared reference for all family-law drafting skills (divorce / judicial separation / restitution of conjugal rights / nullity / maintenance / custody / DV Act / adoption). Holds the anti-pollution rules, the privacy firewall protocol (placeholder substitution for party names, minor identifiers, and intimate-partner details before any AI processing; real-name re-substitution only on the user's local machine in the Refiner step), the AI-style-marker blacklist, citation-discipline rules, and the family-law-specific risk constraints. NOT invoked directly — referenced from every case-type skill in this plugin.
allowed-tools: Read
---

# `_drafting_common` — shared family-law drafting infrastructure

## Privacy firewall

Family-law cases routinely contain the most sensitive personal data in any pleading the plugin will encounter — minor children's names and DOBs, intimate-partner conduct allegations, mental-health disclosures, financial disclosures of both spouses, religious affiliation, and (in DV Act matters) shared-household particulars. The plugin's privacy discipline is therefore stricter for family-law than for any sibling plugin.

**Discipline:**

1. **Reader** substitutes every party name, every minor's name, every intimate-partner detail with structural placeholders (`[Petitioner]`, `[Respondent]`, `[Minor-Child-1]`, `[Minor-Child-2]`, etc.) and stores the placeholder → real-name mapping in the *header section* of `case-facts.md` on the user's local machine only.
2. **Format / Drafter / Verifier / Overseer** operate **only** on the placeholder-substituted content. The underlying AI runtime never holds the real names.
3. **Refiner** is the only agent that touches the placeholder → real-name re-substitution, and it does so by reading the local-only mapping in `case-facts.md` and replacing placeholders in the final `.docx` — strictly on the user's machine.
4. The mapping itself never leaves `case-facts.md`. The plugin's `.gitignore` explicitly excludes `case-facts.md` so it cannot be committed accidentally.

## AI-style-marker blacklist

The Refiner strips the following AI-tell patterns from every draft before output:

- Em-dash (`—`) used as a sentence-internal pause (replaced with semicolon or proper comma-bounded clause)
- Sentence-final *thereby* / *hereby* / *whereby* without a verb attached
- *Moreover*, *furthermore*, *additionally*, *in addition* as sentence-openers
- *Navigate* (used metaphorically), *delve* (used metaphorically), *foster* (used metaphorically), *robust*, *seamless*
- *It is important to note that*, *it should be noted that*, *worthy of note* — replaced with direct prose
- Bullet-list-style structure in Statement of Facts (Statement of Facts must be a chronological narrative, not a list)
- Three-em-dash separators inside the pleading body

## Citation discipline

The Drafter does **not** generate case names + citations from training memory. Every case citation must trace to a user-supplied source in `<case-folder>/laws/` (PDF of the judgment) or `<case-folder>/notes.md` (advocate's existing citation list). Untraceable citations are written as `[CITATION NEEDED]` placeholders for the advocate to fill manually before filing.

The family-law domain has well-known headline cases that the AI's training memory may misquote with confident-sounding but wrong citations:

- *Shilpa Sailesh v. Varun Sreenivasan* — divorce by mutual consent, irretrievable breakdown
- *Joseph Shine v. Union of India* — adultery decriminalisation
- *Vishaka v. State of Rajasthan* — sexual harassment workplace
- *Indra Sarma v. V.K.V. Sarma* — DV Act, live-in relationships
- *Rajnesh v. Neha* — maintenance computation framework
- *Gaurav Nagpal v. Sumedha Nagpal* — custody, welfare-of-child
- *Yashita Sahu v. State of Rajasthan* — custody, international parental child-removal

The Verifier specifically flags any citation to these (or any other) case names whose accompanying year / volume / SCC / SCC-page reference cannot be verified against a user-supplied source.

## Family-law forum awareness

The Drafter respects the State-specific forum architecture:

- **States with Family Courts established under the Family Courts Act 1984** (Maharashtra, Karnataka, Andhra Pradesh, Telangana, Gujarat, Tamil Nadu, Kerala, Madhya Pradesh, Bihar, Rajasthan, Uttar Pradesh, etc.) — matrimonial petitions are filed before the Family Court; cause-title says *IN THE COURT OF THE PRINCIPAL JUDGE, FAMILY COURT AT [PLACE]* or *IN THE COURT OF THE FAMILY JUDGE AT [PLACE]*.
- **States / districts without Family Courts** — matrimonial petitions are filed before the District Court / District Judge; cause-title says *IN THE COURT OF THE DISTRICT JUDGE AT [PLACE]*.

The user's `family-config.md` declares the State and the specific court; the Format agent substitutes accordingly.

## Section 9 Family Courts Act 1984 — mandatory reconciliation

Where the Family Court has jurisdiction, Section 9 of the Family Courts Act 1984 makes reconciliation a mandatory preliminary step. Pleadings filed before the Family Court must aver that the petitioner is willing to attempt reconciliation, or has attempted reconciliation, or that reconciliation is not possible on the facts pleaded. The Drafter includes this averment automatically when `family-config.md` declares the forum is a Family Court under FCA 1984; the Overseer flags any draft that omits the averment.

## Section 89 CPC ADR-reference

For pleadings filed before the District Court (in States without Family Courts), Section 89 CPC requires the Court to attempt ADR — the Drafter includes the averment that the petitioner is willing to participate in ADR (mediation / conciliation / Lok Adalat / arbitration) when ADR is consistent with the relief sought.

## Personal-law applicability check

The Drafter does **not** assume which personal law applies. The user's `family-config.md` carries an explicit `applicable_personal_law` field — Hindu Marriage Act 1955 / Special Marriage Act 1954 / Indian Divorce Act 1869 (Christians) / Parsi Marriage and Divorce Act 1936 / Muslim Personal Law (Shariat) Application Act 1937 + Dissolution of Muslim Marriages Act 1939 / Family Courts Act 1984 (procedural) / Hindu Adoptions and Maintenance Act 1956 / Section 125 BNSS (secular maintenance) / DV Act 2005 (secular). The Verifier checks the parties' religion in `case-facts.md` against the declared applicable law; an HMA divorce filed where one party is Muslim, or an IDA divorce filed where neither party is Christian, gets flagged at Verifier stage and corrected at Refiner.

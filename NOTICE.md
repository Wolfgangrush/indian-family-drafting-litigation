# NOTICE — Provenance and Privilege Statement

This document is part of the public release of the `indian-family-drafting` plugin (v0.1.0-alpha and onwards). It declares the provenance of the plugin's content, in order to address any question about advocate-client privilege, client confidentiality, professional ethics, and personal-data protection that may be raised by any reader, complainant, regulator, or Bar Council disciplinary authority.

The plugin is **family-config-aware**: the universal structural skeleton of any family-law pleading is uniform across personal-law regimes, and the personal-law-specific elements (statutory opening, applicable-section citations, eligibility / capacity / ceremony / consideration rules) are supplied through the case-type skill plus the user's `family-config.md` declaring the applicable personal law for the parties.

This NOTICE is published in plain language so that any reader — practising advocate, Family-Court judge, Magistrate, Bar Council officer, regulator, member of the public, fellow developer — can understand the position without ambiguity.

---

## 1. What this plugin contains

This plugin contains the following categories of content, and **only** the following categories of content:

(a) **Procedural skeletons** — the structural shape of family-law pleadings prescribed by Indian procedural law (cause title, party block, statutory opening, marriage particulars, domestic chronology, grievance narrative, grounds section, prayer, reconciliation / ADR averment, verification, supporting affidavit, list of documents, accompanying applications).

(b) **Formatting conventions** — Registry-compliant text-formatting conventions of Family Courts and District Courts exercising matrimonial jurisdiction — heading style, parties separator, salutation phrasing, verification form, supporting-affidavit form, advocate's signature block.

(c) **Statutory references** — citations to public statutes (Hindu Marriage Act 1955; Special Marriage Act 1954; Indian Divorce Act 1869; Parsi Marriage and Divorce Act 1936; Muslim Personal Law (Shariat) Application Act 1937; Dissolution of Muslim Marriages Act 1939; Family Courts Act 1984; Hindu Adoptions and Maintenance Act 1956; Hindu Minority and Guardianship Act 1956; Guardians and Wards Act 1890; Protection of Women from Domestic Violence Act 2005; Bharatiya Nagarik Suraksha Sanhita 2023 (including Section 125); Maintenance and Welfare of Parents and Senior Citizens Act 2007; Juvenile Justice (Care and Protection of Children) Act 2015; Adoption Regulations 2022; Indian Christian Marriage Act 1872; Muslim Women (Protection of Rights on Marriage) Act 2019; Code of Civil Procedure 1908; and other public enactments).

(d) **Procedural rule references** — citations to public rules of court (Family Court Rules of various States, made under the Family Courts Act 1984; the Civil Manuals of various States; the Maharashtra Family Courts Rules 1988 and corresponding rules of other States; Bar Council of India Standards of Professional Conduct and Etiquette under Section 49(1)(c) of the Advocates Act 1961).

(e) **Generic placeholders** — every variable in every template is marked with a placeholder such as `[Petitioner]`, `[Respondent]`, `[Minor-Child-1]`, `[Date]`, `[Place]`, `[Annexure-A]`. No placeholder is filled with any specific person's name, any specific date, any specific case number, or any specific identifying information.

(f) **Anti-hallucination and privacy-firewall workflow** — six agents (Reader, Format, Drafter, Verifier, Refiner, Overseer) that operate on a case folder supplied by the user. The plugin itself contains no case folder. The Reader applies a privacy-firewall substitution step that replaces every party name, every minor's name, and every intimate-partner detail with structural placeholders before any downstream AI processing.

---

## 2. What this plugin does NOT contain

This plugin does **not** contain any of the following, and has never contained any of the following at any point in any committed version:

(a) **No specific client matter.** No client of the author appears in the plugin, by name, by initials, by case number, by FIR number, by court hall, by date pattern, by relief pattern, or by any other identifying signature.

(b) **No client communications.** No oral or written communication made to the author by or on behalf of any client appears in the plugin in any form, in whole, in summary, in paraphrase, or in pattern.

(c) **No client documents.** No document or instrument with which the author has become acquainted in the course of professional employment as an advocate appears in the plugin, in original, in redacted, in summary, in extract, or in pattern.

(d) **No personal data of any data principal.** The plugin processes no personal data, collects no personal data, stores no personal data. It contains no name, no contact information, no date of birth, no address, no biometric data, no financial data, no health data, no marital-status data, no domestic-violence-event data, no children's data, no other category of personal data as defined under the Digital Personal Data Protection Act 2023 of any identifiable natural person other than the author of this plugin himself (named publicly as the author and publisher).

(e) **No witness testimony, no deposition, no statement, no DV-Act Domestic Incident Report content, no Family Court Counsellor's report, no judgment content** of any specific matter handled by the author or any other advocate.

(f) **No client list, no chamber list, no associate list, no opposing-counsel list, no judge-specific intelligence** of any kind.

(g) **No tracking, no telemetry, no analytics, no opt-in error reporting, no login, no account, no cloud sync.** The plugin runs entirely on the user's machine. The plugin author receives no information about who installs the plugin, who uses it, on what cases, with what facts, with what outcomes.

---

## 3. The legal distinction

Indian law has long recognised a clear distinction between two categories:

(i) **Specific client communications and documents** — protected under Section 132 of the Bharatiya Sakshya Adhiniyam 2023 (formerly Section 126 of the Indian Evidence Act 1872) and under Rule 17 of the Bar Council of India Standards of Professional Conduct and Etiquette. This category is privileged and confidential. An advocate may not disclose it.

(ii) **General professional knowledge of procedural law** — an advocate's accumulated knowledge of how a divorce petition is structured under Section 13 of the Hindu Marriage Act 1955, what the prayer clause of a DV-Act application looks like under Sections 18–22 of the DV Act 2005, what statutory opening a Section 125 BNSS maintenance application requires, how a Family Court Registry expects the verification to appear. This category is the advocate's own professional knowledge. It is not the property of any specific client. It is not privileged.

This plugin operates **entirely within category (ii)**.

Every Indian advocate accumulates this knowledge through years of practice, through study of the Hindu Marriage Act, through study of the Special Marriage Act, through study of the Family Courts Act, through study of the Mulla on the Hindu Law and Mulla on Mahomedan Law and B.M. Gandhi on Family Law, through observation of court practice, through senior-counsel mentorship, through Bar Association continuing education, and through reading the published judgments of the Supreme Court and the High Courts.

The plugin codifies that accumulated procedural knowledge into machine-readable form. It does not codify any client's confidential information.

The plugin is, in this respect, identical in legal character to a published family-law drafting textbook, a continuing legal education handout, or a senior advocate's matrimonial-petition-style lecture. The medium is software. The content is procedural knowledge.

---

## 4. The author's professional position

The author is **Rushikesh R. Mahajan**, Advocate, enrolled with the Bar Council of Maharashtra and Goa, practising before the High Courts of India. The plugin is published under the open-source brand **wolfgang_rush**, which is the author's publishing handle for legal-technology infrastructure; the real-identity accountability declared in this section attaches to the author personally and is not displaced by the use of a publishing handle.

The author retains full enrollment, full responsibility, and full liability under the Advocates Act 1961, the Bar Council of India Rules, and the Standards of Professional Conduct and Etiquette.

The plugin is published as a personal contribution to the open-source legal-technology ecosystem. It is published without any commercial channel, without any solicitation of professional work, without any advertisement of professional services, and without any acceptance of work through this repository.

This NOTICE is filed of record in this open-source repository so that any person who reads, reviews, audits, or assesses this plugin has full transparency about its provenance and its scope from the moment of release.

---

## 5. Verification of clean provenance

The repository owner shall maintain, on a private offline record, a build log demonstrating that every line of every file in the plugin was either:

(a) authored from scratch as procedural skeleton, OR
(b) derived from public statute, public rule, public judgment, or public procedural textbook, OR
(c) derived from the author's own original procedural knowledge as a practitioner.

No line of any file was, at any stage, copied from, paraphrased from, summarised from, or pattern-matched against any specific client matter, client communication, or client document.

This NOTICE is the author's signed declaration of that position.

---

## 6. Reporting concerns

If any reader, regulator, fellow advocate, or member of the public believes any specific content in this plugin is derived from a specific client matter or specific confidential communication, the reader is requested to:

(a) identify the file and line at issue in the plugin,
(b) identify the specific client matter or communication believed to be the source,
(c) explain the basis of the belief,

and raise the concern via a GitHub Issue on this repository.

Concerns raised with these particulars will be investigated and the file or line will be removed or rewritten if the concern is well-founded. Concerns raised without these particulars cannot be acted upon.

---

## 7. Standing instruction to forks and derivatives

Any fork, derivative, downstream redistribution, or commercial integration of this plugin or its content shall preserve this NOTICE in unmodified form, and shall extend the same provenance discipline to any content added in the fork or derivative.

This NOTICE travels with the code under the same MIT licence that governs the source.

---

*Signed and dated by way of public commit history on the repository. The author stands by every line of this notice.*

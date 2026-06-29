# PLAN — food-security-briefs

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

> **Positioning.** The calmest, most trustworthy way to understand the world's food data. We turn
> FAOSTAT and other openly-licensed food/agriculture statistics into (a) clean, reproducible,
> well-documented **derivative datasets** and (b) **plain-language briefs** a non-specialist can
> read without being misled or alarmed. The product is *trust*: every number is traceable to a
> licensed source, every caveat is stated, and nothing is dramatized. We compete with neither the
> source agencies nor the headlines — we are the layer that makes the official numbers usable and
> the explanations honest.

> **In one line.** Official food statistics are rigorous but opaque; news coverage is readable but
> often alarmist and decontextualized. This project fills the gap: rigorous *and* readable, with
> the rigor auditable and the readability non-sensational.

---

## Executive summary

The Food and Agriculture Organization of the United Nations (FAO) publishes FAOSTAT, one of the
largest open statistical corpora on food, agriculture, nutrition, trade, and food security — over
245 countries/territories and 20,000+ indicators, licensed **CC BY 4.0 IGO**. The data is
authoritative but hard to use: indicator definitions are technical, values carry status flags
(official / estimated / imputed / aggregate), food-security headline indicators are published as
multi-year moving averages with uncertainty ranges, country aggregates and reference periods are
easy to misread, and the licence carries an intergovernmental-organization (IGO) attribution +
**non-endorsement** obligation that most reusers ignore. Meanwhile, public-facing coverage of hunger
and food security is frequently alarmist, decontextualized, or politically charged.

This project produces two tightly-coupled deliverables for each topic:

1. **A derivative dataset** — a tidy, normalized, machine-readable extract (Frictionless Tabular
   Data Package + Croissant ML metadata) with decoded status flags, explicit units, ISO/M49 country
   codes, reference periods, preserved uncertainty, full provenance, and a **deterministic,
   reproducible build script** that re-derives it from the licensed source.
2. **A plain-language brief** — a short, accessible, **non-alarmist** explainer of what the data
   says and, just as importantly, what it does *not* say, with indicator definitions, caveats, a
   "not advice" notice, and the required FAO attribution + non-endorsement disclaimer.

The two central design principles are the **licence/provenance gate** (every source verified to be
openly licensed for derivatives, with attribution + non-endorsement recorded) and the
**non-alarmist + statistically-honest editorial standard** (a committed style guide and
statistical-integrity checklist that every brief must pass). Both are blocking gates, not
aspirations.

Risk tier is **medium**. The dominant risks are not technical: they are (1) **statistical
misinterpretation** (conflating chronic undernourishment with acute crises; misreading 3-year
averages, provisional figures, or imputed values), (2) **alarmism / framing harm** (sensational or
politically-charged presentation of a sensitive subject), and (3) **licence/attribution error**
(stripping FAO's required IGO attribution or implying FAO endorsement). The plan front-loads all
three as non-skippable gates with named reviewers, and treats food security as a sensitive civic
topic requiring expert sign-off and a strict "this is information, not advice" framing.

**No partner is yet secured.** The general need is well established; the per-beneficiary delivery
need is **TO BE SECURED**. Until a named partner (educator, newsroom, civil-society group, library,
or humanitarian analyst) confirms they will use the output, all tasks carry `verifiedNeed: false`.

## Problem & beneficiaries

**Who is helped.**
- **Educators and students** (secondary / undergraduate geography, economics, public health,
  development studies) who need a reliable, readable entry point to real food-systems data.
- **Local and independent journalists / data desks** who want correctly-contextualized food
  statistics without misreading the indicators or breaching the licence.
- **Civil-society organizations, community food groups, and small NGOs** who need credible numbers
  for grant applications, advocacy, and public communication but lack a statistics team.
- **Librarians, fact-checkers, and the general informed public** who encounter alarming food/hunger
  claims and need a calm, sourced baseline to check them against.
- **Downstream open-data reusers** who benefit from a tidy, documented, reproducible derivative they
  can build on (with the licence already verified and attribution pre-formatted).

Improving the usability of FAOSTAT also serves FAO's own mission of broadening informed,
correctly-attributed reuse of data it already collects and publishes.

**The verified need.** That FAOSTAT is underused relative to its value because of accessibility and
interpretation barriers is well established. We treat the *general* need as real, but the
**per-brief, per-partner delivery need is TO BE SECURED**: we have not yet confirmed a named
beneficiary who has agreed to use specific briefs/datasets. Until then, individual tasks carry
`verifiedNeed: false`. This honesty matters because "delivered, not merged" requires the output to
be *used by a beneficiary*, not merely produced.

**Partner org.** TO BE SECURED. Candidate channels: open-education networks and OER repositories;
journalism support orgs and data-journalism desks; community-food coalitions and food-bank networks;
public libraries; humanitarian/development analysts (e.g. organizations already in the FAOSTAT reuse
ecosystem). M0 includes explicit partner-outreach work; **no partner is assumed**, and we explicitly
refuse to overstate demand.

## Goals and non-goals

**Goals**
- Produce a reusable **derivative-dataset specification** (Frictionless Data Package + Croissant ML
  metadata + canonical metadata model) and a **brief template** that every topic instantiates.
- For each in-scope topic, deliver a clean derivative dataset **and** a plain-language brief that is
  source-verified, reproducible, correctly licensed, and **passes the non-alarmist + statistical
  integrity gates**.
- Make licence/provenance verification (CC-BY-IGO attribution + non-endorsement) a non-skippable,
  auditable gate.
- Make statistical honesty non-skippable: indicator definitions, reference periods, status flags,
  and published uncertainty are always carried through to the reader.
- Provide a **deterministic build harness** so any third party can re-derive every dataset from the
  licensed source and confirm it matches.
- Reach real beneficiaries: outputs are *used* (taught, cited, republished), not just published.

**Non-goals — constraints as identity.** This project's refusals *are* its brand. A food-data
project that will say anything is worthless; trust comes from what we refuse to do.
- **No forecasting or prediction** beyond what the source itself publishes. We do not model the
  future of hunger.
- **No causal or political attribution.** We never claim a government, policy, company, or group
  "caused" a food outcome. We report what the data measures, with its stated method.
- **No country shaming or rankings designed to provoke.** Comparisons are presented neutrally, with
  context and uncertainty, never as a "worst offenders" leaderboard.
- **No alarmist or sensational framing.** No "starvation crisis," "time bomb," or emotive
  catastrophizing. Calibrated, calm, quantified language only.
- **No advice of any kind** — not dietary, nutritional, medical, agricultural, financial,
  investment, or policy advice. Briefs are information, explicitly labelled "not advice."
- **No acute-crisis / famine classification.** IPC/Cadre Harmonisé acute-food-insecurity phase
  classifications and famine declarations are high-stakes, fast-moving, and alarmism-prone — **out
  of scope** (escalate/refuse rather than dabble). We cover FAOSTAT *structural/long-run* indicators.
- **No individual or household microdata.** Aggregate / country-or-region-level statistics only;
  household-survey microdata and any re-identifiable data are out of scope.
- **No unverified-licence sources.** Anything not confirmed openly licensed for derivatives is
  excluded, not "best-guessed."
- **No relicensing that strips attribution.** We never republish under terms that drop the FAO
  attribution or imply FAO endorsement.
- **No advocacy.** We describe the data; we do not campaign on it.

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Vanity metrics ("briefs written") are explicitly excluded;
the unit of success is a brief/dataset *used by a real beneficiary* with *zero* integrity defects.

| Metric | Baseline | Target (first 6 months) |
| --- | --- | --- |
| Briefs + datasets **used/accepted by a named beneficiary** (last-mile delivered) | 0 | 6 delivered & in use |
| Statistical-misinterpretation defects found in expert review or post-publication | n/a | **0** published statistical errors (target: 0; any found → corrected + postmortem) |
| Licence/attribution errors (missing CC-BY-IGO attribution or non-endorsement) | n/a | **0** (hard gate; 100% of outputs carry correct attribution + disclaimer) |
| Non-alarmist editorial compliance (briefs passing the style-guide review) | n/a | 100% of published briefs pass; 0 published alarmist-framing defects |
| Reproducibility: derivative datasets that re-derive from source within tolerance | n/a | 100% (deterministic build reproduces source totals exactly / within documented tolerance) |
| Readability: briefs hitting the target plain-language reading level | n/a | ≥ 90% at target grade level (see Solution approach) |
| Verifiable external reuse events (taught, cited, republished, linked) | 0 | ≥ 3 with externally verifiable evidence |
| Confirmed delivery partners | 0 | ≥ 1 secured |

Outcome attribution: a "reuse event" must be **externally verifiable** (a citation, a syllabus/lesson
referencing the brief, a republished or linked dataset, a written partner confirmation). Self-reported
reuse does not count.

**Quantifying the soft criteria so DoDs are checkable.**
- *Statistical integrity* is scored per brief via the **statistical-integrity checklist** (M0,
  `statcheck-003`): a pass requires every reported figure to cite indicator code + definition,
  reference period, status flags, and published uncertainty, and to reproduce from the derivative
  dataset. The completed checklist is committed with the brief.
- *Non-alarmism* is scored per brief via the **editorial style-guide review** (M0, `styleguide-002`):
  a committed reviewer checklist (no sensational language, no causal/political attribution, no
  prediction, uncertainty shown, "not advice" present). A single unresolved flag blocks publication.
- *Readability* is measured with a standard readability metric (target ~grade 8–9 / "plain English"),
  recorded per brief; the target is a guideline, never an excuse to drop a necessary caveat.
- *Reproducibility* is measured by re-running the build harness in CI and asserting the output
  checksum and source-total reconciliation match the committed expected values.

## Scope

**In scope**
- **Derivative datasets** from FAOSTAT (and later other verified-open sources): tidy CSV + a
  Frictionless **Tabular Data Package** (`datapackage.json`) + **Croissant ML** JSON-LD metadata,
  with decoded status flags, units, ISO3/M49 geography codes, reference periods, preserved
  uncertainty, and full provenance.
- **Plain-language briefs**: short Markdown/accessible-HTML explainers built *from* the derivative
  dataset, with indicator definitions, caveats, charts (with alt-text), "not advice" notice, and FAO
  attribution + non-endorsement disclaimer.
- A **deterministic build harness** (extract → tidy → reconcile → validate → checksum) so every
  dataset is reproducible from source.
- Licence/provenance/PII triage and recording per source/indicator.
- Translations and accessibility variants of delivered briefs (later phases), with domain + language
  review.

**Candidate topic pool.** FAOSTAT domains span Production (crops/livestock), Trade, Food Balance
Sheets / Supply Utilization Accounts, the **Suite of Food Security Indicators**, Prices, Inputs,
Land Use, and Population. The candidate backlog of *indicators we might brief* lives in
`TOPIC-CATALOG.md` (to be created in M0); each entry is a triage candidate only and becomes a task
only after passing the licence + statistical-suitability + sensitivity gate. The catalog deliberately
biases toward **structural, long-run, low-controversy** indicators first (e.g. food supply trends,
production, dietary energy supply) and explicitly defers or excludes acute-crisis and high-sensitivity
framings.

**Out of scope**
- Forecasting, modelling, scenario projection, or any prediction beyond the source.
- Causal, attributional, political, or advocacy claims.
- Acute food crisis / famine (IPC, Cadre Harmonisé) classification or commentary.
- Any advice (dietary, medical, agricultural, financial, policy).
- Individual/household **microdata** or any re-identifiable data.
- Sources whose licence is unclear, non-derivative, or unverifiable.
- Hosting/mirroring the *entire* source corpus or competing with FAOSTAT as a data portal; we publish
  *focused derivative extracts* with provenance, not a full mirror.
- Automated, unattended publishing; a human reviews and hands off every output.

## Solution approach & architecture

This is a **content + data project with light, reproducible software** (a build harness, validators,
and templates). It is not a hosted service.

**Pipeline (per topic).**
1. **Select & gate** — choose a FAOSTAT domain + indicator(s); run the licence/provenance/PII gate
   (`gate-004`) and the **sensitivity/suitability screen** (is this structural and low-controversy,
   or does it veer into acute-crisis/advice territory → escalate or exclude). Record the decision.
2. **Extract** — pull via the FAOSTAT bulk download / API; record dataset version, release date,
   API endpoint/bulk URL, retrieval timestamp, and source checksum.
3. **Build the derivative dataset** — deterministic transform to tidy long-format CSV: decode status
   flags (e.g. official / estimated / imputed / aggregate), normalize units, attach ISO3 + M49 codes,
   preserve reference periods and published uncertainty (e.g. PoU lower/upper bounds), drop nothing
   silently. Emit `datapackage.json` (Frictionless) + Croissant metadata + a data dictionary.
4. **Statistical QA** — reconcile against source totals/aggregates within a documented tolerance;
   verify flags carried; verify uncertainty preserved; emit a QA report. Run the
   statistical-integrity checklist.
5. **Author the brief** — plain-language explainer built only from the derivative dataset, written to
   the **non-alarmist editorial standard**, including indicator definitions, what the data does *not*
   say, caveats, charts with alt-text, the "not advice" notice, and the FAO attribution +
   non-endorsement disclaimer.
6. **Review (three gates)** — (a) **domain/statistical expert** (food security / agricultural
   economics / nutrition) confirms correctness; (b) **editorial reviewer** confirms the non-alarmist
   standard; (c) **licence/provenance reviewer** confirms attribution + non-endorsement + provenance.
7. **Deliver & record** — a human hands the brief + dataset to the partner/beneficiary and records
   the acceptance/use evidence artifact (`outcomes/<topic-id>.json`).

**Canonical metadata model** (single source of truth; all outputs are projections of it):
`id`, `title`, `source` (`FAOSTAT` | other), `domain`, `indicatorCode`, `indicatorDefinition`,
`geographyLevel` (`global` | `region` | `country`), `referencePeriod`,
`datasetVersion`, `releaseDate`,
`license { id: "CC-BY-4.0-IGO", url, attribution, nonEndorsementNotice, snapshotRef }`,
`provenance { retrievedAt, sourceUrl, apiEndpoint, sourceChecksum, buildScriptRef, outputChecksum }`,
`flagsDecoded[]`, `units`, `uncertainty { published: boolean, type, fields }`,
`pii { present: false, level: "aggregate" }`, `methodologyNotes`, `caveats[]`,
`readabilityScore`, `notAdvice: true`,
`review { statistical, editorial, license }` (each: reviewer, date, pass/flags).

**Locked decisions.**
- **Dataset format is Frictionless Tabular Data Package + Croissant ML metadata** (not a bespoke
  format), so outputs are immediately interoperable and validatable.
- **Builds are deterministic and reproducible.** Every dataset ships the script + pinned source
  version + a committed output checksum; CI re-derives and asserts the checksum. No hand-edited data.
- **The brief is built from the derivative dataset, never from memory or the raw portal.** Every
  number in a brief must resolve to a row in the committed dataset. No un-sourced figures.
- **Three independent review gates** (statistical, editorial, licence) are all blocking. None may be
  waived.
- **"Not advice" + non-alarmist standard is enforced by a committed checklist**, not reviewer vibe.
- **FAOSTAT first; other sources only after per-source licence verification** (`source-017`).
- **Structural/long-run indicators first; acute-crisis framing excluded** by the sensitivity screen.

**Tech stack.** TypeScript, ESM, pnpm workspaces (Elyos convention). The build harness, validators,
and Croissant/Frictionless emitters are small Node packages with minimal dependencies. Datasets are
CSV + JSON/JSON-LD; briefs are Markdown rendered to accessible HTML/PDF. Charts are generated
deterministically with committed alt-text. No runtime service; everything runs locally or in CI.
Code is **MIT**; datasets and briefs are **CC-BY-4.0** (preserving FAO attribution + non-endorsement).

**Pinned spec versions** (recorded in the canonical model's `specVersions`, bumped only by a
deliberate task): **Frictionless Tabular Data Package** v1 / Data Package spec; **Croissant ML** v1.0;
**ISO 3166-1 alpha-3** + **UN M49** for geography; **SPDX** identifiers for licences. The pinned
**FAOSTAT dataset version + release date** is recorded per dataset (FAOSTAT revises and re-releases).

## Data, licensing & compliance

**This is the critical section.** Our deliverable is a *derivative* of others' data, so licensing and
attribution must be exact and conservative.

**Primary source licence — FAOSTAT.** FAOSTAT statistical data is published under **Creative Commons
Attribution 4.0 International IGO (CC BY 4.0 IGO)**. This licence:
- **Permits** copying, redistribution, adaptation, and derivative works (including for commercial
  use) — so derivative datasets and briefs are allowed.
- **Requires attribution** to FAO with a link to the source and an indication if changes were made.
- **Requires a non-endorsement disclaimer** for adaptations: the adaptation must state it is an
  adaptation and that FAO does **not** endorse it and is **not** responsible for its content. We use
  FAO's standard wording, e.g.: *"This is an adaptation of an original work by FAO, [dataset/title,
  year]. Views and opinions expressed in this adaptation are the sole responsibility of the author(s)
  of the adaptation and are not endorsed by FAO."*
- **Carries an IGO mediation/arbitration clause** (disputes under the licence are subject to
  WIPO/UNCITRAL mediation/arbitration). This is recorded in the licence note; it does not restrict
  our compliant reuse.

**Per-indicator caveat (binding).** A handful of FAOSTAT domains incorporate **third-party data with
their own terms** (e.g. certain population, price, or trade inputs sourced from other providers). The
gate (`gate-004`) **must** check the specific domain/indicator's metadata for third-party-data
notices and exclude or escalate any indicator whose underlying data is not cleanly CC-BY-IGO. We do
not assume the whole corpus is uniformly licensed.

**"Similar" / additional sources.** Any non-FAOSTAT source (e.g. World Bank Open Data — CC-BY-4.0;
Our World in Data — CC-BY-4.0; UN World Population Prospects) is admitted **only** after per-source
licence verification (`source-017`) confirms it is openly licensed for derivatives with recorded
attribution terms. Sources with non-commercial, share-alike-incompatible, non-derivative, or unclear
terms — and **all famine/acute-crisis classification feeds (IPC, Cadre Harmonisé)** and any feed of
questionable provenance or licence — are **excluded**.

**Objective "permits derivatives" acceptance criterion.** A source/indicator PASSes only if its
licence is on the accepted list **and** an explicit `license.permitsDerivatives: true` is recorded
with a cited clause/URL, **and** the attribution string + non-endorsement disclaimer are recorded.
Missing evidence, third-party-data entanglement, or an unparseable licence = FLAG/EXCLUDE, never
default-allow.

**Provenance model.** Every dataset records: source URL, API endpoint / bulk URL, publisher (FAO),
retrieval timestamp, **FAOSTAT dataset version + release date**, source checksum, the deterministic
**build script reference + output checksum**, licence id + URL + a **captured snapshot** of the
licence/terms page (committed copy + SHA-256 + Wayback save URL), the attribution string, and the
non-endorsement disclaimer. Provenance is part of the committed deliverable and is what makes the
derivative auditable and reproducible.

**Privacy / PII stance.** FAOSTAT is **aggregate** (country/region/global) statistics — no personal
data. The gate still runs a PII screen and **hard-excludes any microdata / household-survey /
individual-level source** and anything re-identifiable. `pii.level` is recorded as `aggregate` and
the gate must confirm it. We never ingest individual-level data; if a candidate source turns out to
be microdata, it is excluded.

**Attribution requirements (every output).** Every dataset and brief carries: the FAO attribution
string with source link and "changes made" indication, the **non-endorsement disclaimer**, the
indicator code + reference period, and a clear statement that the *derivative* (not the source) is
our contribution. Our derivative datasets and briefs are licensed **CC-BY-4.0**; the build/validator
**code is MIT**. We do not relicense FAO's data or drop its attribution.

## Quality, review & risk gates

**Risk tier: medium** (per the portfolio roadmap). Food security is a sensitive civic/economic topic;
the risk is factual accuracy, framing, and licence — not code.

**Required review before a deed is "done" (all three are blocking gates):**
1. **Domain / statistical expert reviewer** (food security analyst, agricultural economist, or
   public-health nutrition specialist with relevant credentials/experience): confirms indicator
   interpretation, reference periods, flag handling, and uncertainty are correct, and that nothing
   strays into advice or acute-crisis territory. Mandatory for every brief.
2. **Editorial reviewer** (non-alarmist standard): confirms the style guide is met — no sensational
   language, no causal/political attribution, no prediction, uncertainty visible, "not advice"
   present, comparisons neutral.
3. **Licence / provenance reviewer**: confirms CC-BY-IGO attribution + non-endorsement disclaimer +
   provenance + reproducibility are present and correct, and that no third-party-data entanglement or
   PII slipped through.

A task touching any health/nutrition or acute-crisis edge is either **escalated** (treated as
high-stakes, requiring credentialed expert sign-off and possibly out of scope) or **excluded** — it
is never quietly published at medium tier.

**Test fixtures & golden files (so "CI green" means something).** Validators and emitters ship
committed test assets exercised in CI using **synthetic or small public fixtures only**:
- **Croissant / Frictionless emitters** — golden canonical-model → expected output pairs, plus
  known-valid and deliberately-malformed cases, validated against the pinned specs.
- **Build harness** — a fixture extract with a known expected output checksum and source-total
  reconciliation; CI re-derives and asserts equality (proves reproducibility).
- **Flag-decoder / uncertainty-preservation** — unit tests asserting status flags and published
  ranges survive the transform intact.

**Definition of Shipped.** A derivative dataset + brief that (1) passed the licence/provenance/PII
gate (CC-BY-IGO, attribution + non-endorsement, aggregate-only, no third-party-data entanglement);
(2) passed the statistical-integrity checklist and **domain expert** sign-off; (3) passed the
**editorial non-alarmist** review; (4) is **reproducible** (build re-derives from source in CI, with
committed checksums); (5) carries correct attribution + "not advice" notice; and (6) has been
**handed to and accepted/used by a named beneficiary**, with the Steward's acceptance-evidence
artifact recorded. Producing the output is *not* shipped; recorded beneficiary use is.

## Roadmap & milestones

**M0 — Foundation & cold-start (thin)**
- Goal: build the reusable standard + harness and prove the end-to-end flow on **one low-controversy
  pilot**; secure reviewers; begin partner outreach.
- **Cold-start de-risking.** The pilot indicator is chosen to be structural, long-run, and
  uncontroversial (e.g. global/regional **food supply (kcal/capita/day)** or **cereal production**
  trends) so the first brief exercises every gate without touching sensitive framing. Delivery uses a
  realistic path first: (a) an **informal channel** (an educator/newsroom/library who agreed to
  review), failing that (b) a **self-serve fallback** we can publish ourselves (an open repo + Zenodo
  DOI for the dataset and a brief on an open page) so M0 yields a real *delivered* outcome, not a
  "produced, unused" one.
- Exit criteria: (1) non-alarmist editorial style guide + "not advice" framing published;
  (2) statistical-integrity checklist published; (3) licence/provenance/PII gate (CC-BY-IGO
  attribution + non-endorsement + sensitivity screen) published and applied to one topic;
  (4) derivative-dataset spec (Frictionless + Croissant + canonical model) + deterministic build
  harness green in CI with golden fixtures; (5) **domain expert + editorial reviewers named**
  (blocking roles filled before pilot review); (6) one pilot brief + dataset delivered end-to-end and
  **used/accepted** via an informal channel or self-serve fallback (evidence recorded) — or submitted
  with the blocker surfaced; (7) ≥ 1 partner-outreach thread opened.

**M1 — Gates hardened + first deliveries**
- Goal: make the gates rigorous and get real deliveries used.
- Exit criteria: (1) licence/provenance + statistical-integrity gates codified as reviewable
  artifacts and applied to ≥ 3 indicators; (2) ≥ 2 briefs + datasets reviewed (all three gates) and
  **delivered/in use**; (3) ≥ 1 confirmed delivery partner; (4) licence-snapshot capture working in
  the decided format; (5) reproducibility harness verified on every delivered dataset.

**M2 — Scale, accessibility & multilingual**
- Goal: scale the template across indicators/geographies and widen reach.
- Exit criteria: (1) scalable brief template with accessible charts (alt-text) + readability target
  met; (2) ≥ 1 brief translated with domain + language review; (3) ≥ 5 briefs + datasets delivered
  cumulatively; (4) ≥ 1 additional non-FAOSTAT source licence-verified (`source-017`) or formally
  deferred; (5) median per-brief effort reduced vs. the M0/M1 baseline.

**M3 — Outcomes & sustainability**
- Goal: demonstrate real downstream use and a maintenance model tied to the FAOSTAT release cycle.
- Exit criteria: (1) ≥ 3 verifiable external reuse events; (2) ≥ 8 briefs + datasets delivered
  cumulatively across ≥ 3 distinct FAOSTAT domains; (3) documented refresh process tied to FAOSTAT
  re-releases + drift detection; (4) steward identified for ongoing partner liaison + outcome tracking.

Dependencies: M1 depends on the M0 standard + harness; M2 scaling depends on M1's reviewed template;
M3 outcomes depend on a body of delivered briefs from M1–M2.

## Work breakdown

The itemized, schema-mapped backlog lives in `TASKS.md`, organized by the milestones above. Each
milestone has a task table (`ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer`),
acceptance criteria for the most important tasks, and a milestone Definition of Done. A backlog of
sized-but-unscheduled tasks and one complete, schema-valid example Task JSON are included there. The
candidate **topic backlog** that feeds per-topic brief/dataset tasks lives in `TOPIC-CATALOG.md`
(created in M0); per-topic tasks are instantiated from it only after passing the gate.

## Governance, roles & stakeholders

- **Maintainer (Owner):** TBD — owns the standard, harness, triage, and backlog.
- **Domain / statistical expert reviewer:** TBD (name TO BE SECURED) — **mandatory, non-skippable**
  gatekeeper for every brief; must be filled **before the M0 pilot (`pilot-008`) is reviewed**. May
  rotate among ≥ 2 qualified reviewers to avoid a bottleneck, but at least one named, qualified
  reviewer must exist at all times or brief authoring halts. Until named, all tasks remain
  `verifiedNeed: false` and no brief can be shipped.
- **Editorial (non-alarmist) reviewer:** TBD — enforces the style guide; blocking for every brief.
- **Licence / provenance reviewer:** TBD — confirms CC-BY-IGO attribution + non-endorsement +
  provenance + PII screen; blocking for every dataset/brief.
- **Technical reviewer(s):** rotation of contributors who verify the harness, emitters, validators,
  and reproducibility (CI green).
- **Steward (last-mile owner):** TBD — owns relationships with partners/beneficiaries and confirms
  use (the "delivered" signal). Critical because Definition of Shipped is *use*, not production.
- **Partner / requestor:** TO BE SECURED — named educator(s), newsroom(s), civil-society group(s),
  or library/analyst beneficiary.

## Dependencies & integrations

- **Primary dataset:** FAOSTAT (FAO) — CC BY 4.0 IGO. Accessed via bulk download / API; specific
  domains/indicators TO BE SELECTED via triage.
- **Additional sources (only after `source-017` verification):** World Bank Open Data (CC-BY-4.0),
  Our World in Data (CC-BY-4.0), UN WPP — each individually verified; none assumed.
- **External standards/specs (pinned):** Frictionless Tabular Data Package, Croissant ML v1.0,
  ISO 3166-1 alpha-3, UN M49, SPDX licence identifiers. Versions recorded in `specVersions`.
- **External services:** Zenodo (self-serve dataset DOI / fallback delivery), an open repo host
  (GitHub) and Wayback Machine (licence snapshots).
- **Elyos pieces:** Task JSON schema (`packages/schema`), the donated-lane CLI workspace/PR flow
  (`packages/cli`), the good-deed definition + refusal guardrails. No funded-lane/runner dependency
  (this project is donated lane).

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
| --- | --- | --- | --- | --- |
| Statistical misinterpretation (e.g. conflating chronic undernourishment with acute crisis; misreading 3-yr averages / imputed values) | Medium | High | Statistical-integrity checklist + mandatory domain-expert sign-off; every figure resolves to a dataset row; uncertainty + flags carried through | Domain expert reviewer |
| Alarmist / sensational / politically-charged framing of a sensitive topic | Medium | High | Non-alarmist editorial style guide as a committed blocking checklist; editorial reviewer; explicit non-goals (no prediction, no attribution, no shaming) | Editorial reviewer |
| Licence/attribution error — stripping FAO attribution or implying endorsement | Medium | High | Licence gate; mandatory attribution + non-endorsement disclaimer template; licence reviewer; snapshot recorded | Licence reviewer |
| Third-party-data entanglement inside a FAOSTAT domain (not uniformly CC-BY-IGO) | Medium | Medium | Per-indicator licence check in the gate; exclude/escalate any non-clean indicator | Licence reviewer |
| Ingesting microdata / re-identifiable data | Low | High | PII screen in gate; aggregate-only rule; hard-exclude microdata sources | Licence reviewer |
| Source data revised/re-released → brief becomes stale or inconsistent | Medium | Medium | Pin dataset version + release date; reproducible build; refresh process (M3) tied to FAOSTAT cycle; drift detection | Maintainer |
| Non-reproducible / hand-edited data undermines trust | Low | High | Deterministic build harness + committed checksums asserted in CI; no hand-edited datasets | Maintainer |
| Output produced but never used (fails "delivered") | Medium | High | M0 partner outreach + self-serve fallback; steward role; `verifiedNeed:false` until partner secured | Steward |
| Scope creep into forecasting, advice, advocacy, or acute-crisis commentary | Medium | High | Explicit non-goals; sensitivity screen in gate; reviewers reject out-of-scope framing | Maintainer |
| Spec drift (Croissant/Frictionless) | Low | Low | Canonical-model-first; emitters pinned to spec versions; isolated version-bump task | Maintainer |

## Security & privacy

- **Threat surface is small** (no runtime service, no data hosting beyond focused derivative
  extracts). Main surfaces are CI and the published derivative datasets/briefs.
- **Secrets handling:** the FAOSTAT API/bulk download needs no credentials by default. If any
  source ever requires a token, it is supplied by the human and **never** written into logs,
  receipts, or committed files (per Elyos rules).
- **PII:** the dominant concern is upstream — we restrict to aggregate sources and hard-exclude
  microdata. We never store or process individual-level data.
- **Integrity/abuse prevention:** refuse and flag any task that would steer briefs toward alarmism,
  political targeting, advocacy, advice, country shaming, or laundering an unverified-licence source
  as open. Every brief's figures must be reproducible from the committed dataset — this defends
  against fabricated or cherry-picked numbers.
- **Misinformation defense:** because food security is a misinformation magnet, the non-alarmist +
  statistical-integrity gates and full provenance are themselves the abuse mitigation — the output is
  designed to be *checkable*, which is the opposite of disinformation.

## Sustainability & maintenance

- **Post-delivery ownership:** the steward maintains partner relationships and outcome tracking; the
  maintainer keeps the harness, emitters, and template current with spec and FAOSTAT changes.
- **Refresh:** because FAOSTAT revises and re-releases, each dataset records its pinned version; the
  refresh process (M3) detects when a source has been re-released and schedules a `maintenance` task
  to rebuild + re-review the affected brief (revisions can change the story, so a refresh is a
  re-review, not a silent update).
- **Outcome tracking:** the steward records use/acceptance and verifiable reuse events against the
  success metrics; reviewed each milestone.

## Open questions

- Which beneficiary type (educators, newsrooms, civil-society groups, libraries) is the first
  confirmed delivery partner, and via which channel?
- What is the exact published readability target (grade level / metric) for briefs?
- What numeric tolerance counts as "reproduces source totals" given FAOSTAT's rounding and imputation?
- For multilingual briefs, do we translate in-house with a language+domain reviewer, or partner with
  an existing translation effort (e.g. an Elyos translation project)?
- Where is the canonical licence-snapshot stored — assume the same decided format as sibling projects
  (committed copy + SHA-256 + Wayback URL); to be confirmed before `snapshot-009`.
- Do we ever admit a single, well-licensed *acute*-indicator under escalated expert review, or keep a
  blanket exclusion? (Default: blanket exclusion.)

## References

- Elyos work rules — `C:\code\elyos\CLAUDE.md`
- Good Deed Definition + risk tiers — `C:\code\elyos\docs\good-deed-definition.md`
- Task JSON schema — `C:\code\elyos\packages\schema\src\schemas.ts`
- Portfolio roadmap (food-security-briefs entry) — `C:\code\elyos\planning\ROADMAP.md`
- Sibling data project (conventions reference) — `C:\code\elyos\planning\projects\open-data-datasheets\PLAN.md`
- FAOSTAT — FAO statistical database; CC BY 4.0 IGO terms of use
- Creative Commons Attribution 4.0 International IGO (CC BY 4.0 IGO)
- Frictionless Data — Tabular Data Package / Data Package specification
- Croissant ML metadata format specification (MLCommons)
- ISO 3166-1 alpha-3; UN M49 standard area codes; SPDX license list

---

## Appendix A — Improvements applied

Twenty-five specific refinements made to the first draft of this plan and `TASKS.md`, each now
reflected in the documents above.

1. **Three independent blocking gates, not one.** Split review into domain-expert, editorial
   (non-alarmism), and licence/provenance gates so a brief cannot ship correct-but-alarmist or
   readable-but-wrong. (Quality gates; Governance.)
2. **Non-alarmism made enforceable, not aspirational.** Added a committed editorial style guide +
   reviewer checklist (`styleguide-002`) that yields a per-brief pass/flag artifact; one flag blocks
   publication. (Success metrics; M0.)
3. **Every figure must resolve to a dataset row.** Added the rule that a brief may contain no number
   that is not present in the committed derivative dataset — defeats cherry-picking and fabrication.
   (Solution approach; `statcheck-003`.)
4. **Deterministic, checksum-verified reproducibility.** Builds re-derive from the pinned source in
   CI and assert committed checksums + source-total reconciliation; no hand-edited datasets.
5. **Per-indicator third-party-data check.** Recognized FAOSTAT is not uniformly CC-BY-IGO across all
   domains; the gate excludes/escalates indicators with entangled third-party data. (Data/licensing.)
6. **Mandatory CC-BY-IGO non-endorsement disclaimer** with concrete FAO wording, plus the IGO
   mediation/arbitration note — a requirement most reusers miss. (Data/licensing.)
7. **Acute-crisis / famine (IPC, Cadre Harmonisé) explicitly out of scope.** A sensitivity screen in
   the gate excludes the highest-alarmism, fastest-moving, most license-fraught material.
8. **"Not advice" framing baked into the template and notice wording**, covering dietary, medical,
   agricultural, financial, and policy advice. (Non-goals; `styleguide-002`.)
9. **Microdata hard-excluded; aggregate-only.** PII screen records `pii.level: aggregate` and rejects
   household-survey/re-identifiable sources. (Data/licensing; Security & privacy.)
10. **Constraints-as-identity section** (positioning + explicit non-goals as the brand), matching the
    exemplar's depth. (Goals and non-goals.)
11. **Locked decisions enumerated** (Frictionless+Croissant format, deterministic builds, brief-from-
    dataset, FAOSTAT-first, structural-indicators-first). (Solution approach.)
12. **Uncertainty is preserved end-to-end** — e.g. PoU lower/upper bounds carried from source through
    the dataset into the brief, never dropped to a single point estimate. (`statcheck-003`.)
13. **Status-flag decoding is explicit** (official/estimated/imputed/aggregate) and unit-tested, so
    estimated values are never presented as observed. (Quality gates.)
14. **Cold-start self-serve fallback** (Zenodo DOI + open repo) so M0 yields a real *delivered*
    outcome even with no partner secured. (Roadmap M0.)
15. **Low-controversy pilot indicator chosen deliberately** (food supply kcal/capita/day or cereal
    production) to exercise gates without sensitive framing. (Roadmap M0; `pilot-008`.)
16. **Honest `verifiedNeed: false` everywhere** until a named beneficiary confirms use; partner status
    is TO BE SECURED, not invented. (Problem & beneficiaries; TASKS mapping.)
17. **Outcome-based success metrics** with zero-defect targets for statistical, licence, and alarmism
    errors — and self-reported reuse explicitly disallowed. (Success metrics.)
18. **Refresh = re-review, not silent update.** Because FAOSTAT revises and re-releases, a refresh
    triggers a rebuild *and* full re-review of the affected brief. (Sustainability; `refresh-019`.)
19. **Readability target quantified** (≈ grade 8–9 plain English), measured per brief, but never an
    excuse to drop a caveat. (Success metrics.)
20. **Geography normalized to ISO3 + UN M49**, removing a common source of country-matching errors in
    FAOSTAT reuse. (Solution approach; canonical model.)
21. **Accessibility built in** — charts ship committed alt-text and accessible contrast (`template-014`).
22. **Additional sources gated behind explicit verification** (`source-017`); World Bank/OWID admitted
    only after per-source licence checks; nothing assumed. (Dependencies; Data/licensing.)
23. **Licence-text snapshot format pinned** (committed copy + SHA-256 + Wayback URL), consistent with
    sibling Elyos projects, decided before `snapshot-009`. (Provenance; Open questions.)
24. **Risk table assigns an owner to every risk** and elevates statistical-misinterpretation and
    alarmism to High impact. (Risks & mitigations.)
25. **Schema-faithful TASKS.md**: correct `deliverable` use (`dataset` for derivative data, `document`
    for briefs/specs, `pr` for code, `translation` for i18n), a complete schema-valid example Task
    JSON, and a task rollup with type/risk distribution. (TASKS.md.)

---

## Review sign-off

**Completeness.** All 17 required PLAN.md H2 sections are present and in order. TASKS.md contains the
Elyos field-mapping, four milestone sections matching the roadmap (M0–M3), per-milestone task tables,
acceptance criteria for the key tasks, a per-milestone Definition of Done, a backlog, a schema-valid
example Task JSON, and a rollup. 20 scheduled + 5 backlog tasks (within the ~12–20 robust-backlog
guidance, plus future work).

**Correctness — schema.** The example Task JSON includes every `required` field from
`packages/schema/src/schemas.ts` (`id, title, project, type, lane, priority, domain, riskTier, urgent,
deliverable, tokenEstimate, status, context, objective, acceptanceCriteria, output, verifiedNeed`),
uses only enum-valid values, has `acceptanceCriteria` with ≥ 1 item, and omits `fundedBudgetUsd`
correctly (donated lane). `deliverable` values across tasks are all within
`pr | dataset | document | translation`.

**Correctness — guardrails.** License/provenance (CC-BY-IGO + non-endorsement + per-indicator
third-party check), privacy (aggregate-only, microdata excluded), non-partisan/non-alarmist (style
guide + sensitivity screen), and "not advice" + expert review (domain-expert gate, acute-crisis
excluded) are all addressed and made into blocking gates. Reproducibility/provenance gates (research
moonshot guidance) are applied via the deterministic harness, committed checksums, and licence
snapshots. No primary for-profit benefit; outputs are open (CC-BY-4.0 / MIT).

**Fixes applied during review.** (a) Ensured `riskTier` is `medium` for sensitive writing/spec tasks
(`styleguide-002`, `statcheck-003`, `gate-004`) rather than defaulting to `low`. (b) Confirmed the
pilot's `deliverable` is `dataset` (it ships a derivative dataset alongside the brief) and that
brief-only tasks still pair a dataset. (c) Reconciled the rollup counts with the tables.

**Needs a human decision (flagged, not invented).** (1) Secure the first delivery partner /
beneficiary and channel. (2) Name the domain-expert, editorial, and licence reviewers (blocking before
the M0 pilot review). (3) Confirm the readability target and the numeric reconciliation tolerance.
(4) Confirm the licence-snapshot format matches the org standard. Until (1)–(2) are resolved, every
task remains `verifiedNeed: false` and no brief ships.

**Status:** Plan is internally consistent and ready for maintainer review. No `high`-risk tasks are
scheduled by design; any acute-crisis or advice-adjacent topic must be escalated or excluded, never
shipped at `medium`.


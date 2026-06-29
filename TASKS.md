# TASKS — food-security-briefs

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against
`packages/schema/src/schemas.ts`. Field mapping:

- `id` — stable slug from the tables (e.g. `food-security-briefs-styleguide-002`).
- `title` — the table's Title.
- `project` — `food-security-briefs`.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (per table).
- `lane` — `donated` for all tasks here (no funded escrow). A funded task would add `fundedBudgetUsd`.
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["food-security","agriculture","public-data","open-knowledge"]`.
- `riskTier` — `low | medium | high`. Briefs are `medium` (sensitive topic + interpretation +
  framing). Pure-tooling/spec tasks are `low`. Anything edging into advice/acute-crisis is escalated
  or excluded — never silently shipped at `medium`.
- `urgent` — boolean; `false` for all current tasks.
- `deliverable` — `pr | dataset | document | translation`. Derivative datasets → `dataset`; briefs and
  specs/checklists → `document`; tooling → `pr`; translations → `translation`.
- `tokenEstimate` — `small | medium | large` (Size column).
- `status` — `open | in-progress | review | delivered | done`; all start `open`.
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — per task.
- `requestor` — **TO BE SECURED** until a partner is confirmed.
- `verifiedNeed` — **`false`** until a named beneficiary agrees to use the output (general need is
  real; per-task delivery need is unproven).
- `outputLicense` — `CC-BY-4.0` for datasets and briefs (preserving FAO attribution +
  non-endorsement); `MIT` for code (harness/validators/emitters).

---

## Milestone M0 — Foundation & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| food-security-briefs-reviewer-001 | Name/secure domain-expert + editorial reviewers (blocking gate roles) | research | small | low | document | — | Maintainer |
| food-security-briefs-styleguide-002 | Non-alarmist editorial style guide + "not advice" framing | writing | small | medium | document | — | Editorial |
| food-security-briefs-statcheck-003 | Statistical-integrity checklist (definitions, flags, periods, uncertainty) | design-spec | small | medium | document | — | Domain expert |
| food-security-briefs-gate-004 | Licence/provenance/PII + sensitivity gate (CC-BY-IGO) | design-spec | small | medium | document | — | Licence |
| food-security-briefs-dataspec-005 | Derivative-dataset spec (Frictionless + Croissant + canonical model) | design-spec | small | low | document | — | Technical |
| food-security-briefs-harness-006 | Deterministic build harness (extract→tidy→reconcile→checksum) | code | medium | low | pr | dataspec-005 | Technical |
| food-security-briefs-outreach-007 | Partner/beneficiary outreach + indicator shortlist (TOPIC-CATALOG.md) | research | small | low | document | — | Maintainer |
| food-security-briefs-pilot-008 | Pilot brief + derivative dataset, end-to-end (low-controversy indicator) | data | medium | medium | dataset | reviewer-001, styleguide-002, statcheck-003, gate-004, dataspec-005, harness-006, outreach-007 | Domain expert, Editorial, Licence |

**Acceptance criteria — key tasks**

- **styleguide-002 (non-alarmist editorial style guide)**
  - [ ] Bans sensational/emotive framing, causal/political attribution, prediction beyond source,
        country-shaming/rankings, and any advice (dietary/medical/agricultural/financial/policy).
  - [ ] Requires every brief to show indicator definition, reference period, status flags, published
        uncertainty, and an explicit "what the data does **not** say" section.
  - [ ] Specifies the FAO attribution + non-endorsement disclaimer placement and the "not advice"
        notice wording.
  - [ ] Sets a published readability target (grade level / metric) and a reviewer checklist that
        yields a committed pass/flag artifact per brief; one unresolved flag blocks publication.
  - [ ] Output licensed CC-BY-4.0.

- **statcheck-003 (statistical-integrity checklist)**
  - [ ] Enumerates how to handle FAOSTAT status flags (official/estimated/imputed/aggregate),
        3-year moving averages (e.g. PoU), provisional vs. final, and imputed values.
  - [ ] Requires every figure in a brief to resolve to a row in the committed derivative dataset.
  - [ ] Requires published uncertainty (e.g. PoU lower/upper bounds) to be carried into the brief.
  - [ ] Distinguishes chronic/structural indicators from acute-crisis concepts (which are out of
        scope) so the two are never conflated.
  - [ ] Produces a committed, reviewable checklist artifact per brief.

- **gate-004 (licence/provenance/PII + sensitivity gate)**
  - [ ] Verifies the indicator is cleanly **CC BY 4.0 IGO**, records `permitsDerivatives: true` with a
        cited clause/URL, and records the attribution string + non-endorsement disclaimer.
  - [ ] **Per-indicator third-party-data check**: excludes/escalates any FAOSTAT domain whose
        underlying data is not uniformly CC-BY-IGO.
  - [ ] PII screen: confirms `pii.level: aggregate`; hard-excludes microdata / re-identifiable data.
  - [ ] **Sensitivity screen**: excludes acute-crisis/famine framing and any advice-adjacent topic;
        flags anything needing escalation to credentialed expert sign-off.
  - [ ] Records licence id + URL + snapshot reference (committed copy + SHA-256 + Wayback URL).
  - [ ] Produces a committed PASS/FLAG/EXCLUDE artifact per topic.

- **pilot-008 (pilot brief + dataset, end-to-end)**
  - [ ] Pilot is a structural, low-controversy indicator (e.g. food supply kcal/capita/day or cereal
        production trends), selected with a realistic delivery path (informal channel or self-serve
        Zenodo/open-repo fallback) so a real *delivered* outcome is reachable.
  - [ ] Passed gate-004 (CC-BY-IGO, attribution + non-endorsement, aggregate-only, no third-party
        entanglement) with the artifact committed.
  - [ ] Derivative dataset built by the deterministic harness: tidy CSV + `datapackage.json` +
        Croissant metadata + data dictionary; flags decoded; uncertainty preserved; source totals
        reconciled; output checksum committed and re-derived in CI.
  - [ ] Brief passes the statistical-integrity checklist (every figure resolves to a dataset row) and
        the non-alarmist editorial review; readability target met; "not advice" notice present.
  - [ ] All three gates signed off (domain expert, editorial, licence) with committed artifacts.
  - [ ] **Delivered/used** via the chosen channel with the Steward's acceptance-evidence artifact
        (`outcomes/<topic-id>.json`) recorded — or submitted with the blocker surfaced.

**M0 Definition of Done:** domain-expert + editorial reviewers named (blocking roles filled before
pilot review); style guide + statistical-integrity checklist + licence/provenance/PII+sensitivity
gate + derivative-dataset spec published; deterministic build harness green in CI with golden
fixtures + checksum reconciliation; one pilot brief + dataset delivered end-to-end through all three
gates and **used/accepted** (evidence recorded) — or submitted with the blocker surfaced; ≥ 1
partner-outreach thread opened.

---

## Milestone M1 — Gates hardened + first deliveries

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| food-security-briefs-snapshot-009 | Licence/terms snapshot capture in provenance flow | code | small | low | pr | dataspec-005, gate-004 | Technical |
| food-security-briefs-triage-010 | Triage 3 indicators through gate + statcheck | research | medium | medium | document | gate-004, statcheck-003, outreach-007 | Licence, Domain expert |
| food-security-briefs-brief-011 | Brief + derivative dataset #2 | data | medium | medium | dataset | pilot-008, triage-010 | Domain expert, Editorial, Licence |
| food-security-briefs-brief-012 | Brief + derivative dataset #3 | data | medium | medium | dataset | pilot-008, triage-010 | Domain expert, Editorial, Licence |
| food-security-briefs-partner-013 | Secure first confirmed delivery partner | research | small | low | document | outreach-007 | Steward |

**Acceptance criteria — key tasks**

- **snapshot-009 (licence/terms snapshot capture)**
  - [ ] Implements the decided storage format: committed copy of the FAOSTAT terms/licence page +
        SHA-256 hash + Wayback Machine save URL; `license.snapshotRef` records path + hash + timestamp.
  - [ ] Code MIT-licensed; tests + CI green; no credentials embedded.

- **triage-010 (triage 3 indicators)**
  - [ ] Three indicators evaluated with a committed gate artifact each (PASS/FLAG/EXCLUDE + rationale),
        including the third-party-data check, PII screen, and sensitivity screen.
  - [ ] Each PASS records licence id/URL/snapshot, `permitsDerivatives: true`, attribution +
        non-endorsement, and a statistical-suitability note (flags/periods/uncertainty handled).
  - [ ] Any acute-crisis, advice-adjacent, or non-clean-licence indicator is EXCLUDED/escalated with
        the concern surfaced.

- **brief-011 / brief-012 (briefs + datasets #2, #3)** *(shared pattern)*
  - [ ] Source passed gate-004; dataset built by the harness (reproducible, checksum in CI).
  - [ ] All three review gates signed off; every figure resolves to a dataset row.
  - [ ] Attribution + non-endorsement + "not advice" present; readability target met.
  - [ ] Delivered/used with acceptance-evidence artifact recorded; `verifiedNeed` flips to `true`
        only once a named partner confirms use.

- **partner-013 (first confirmed delivery partner)**
  - [ ] A named beneficiary confirms they will use the briefs/datasets (with channel documented).
  - [ ] Tasks for that partner updated to `verifiedNeed: true` with `requestor` set.

**M1 Definition of Done:** gates applied to ≥ 3 indicators with committed artifacts; ≥ 2 briefs +
datasets delivered/in use (all three gates, acceptance evidence recorded); ≥ 1 confirmed partner;
licence-snapshot capture working in the decided format; reproducibility verified on every delivered
dataset.

---

## Milestone M2 — Scale, accessibility & multilingual

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| food-security-briefs-template-014 | Scalable brief template + accessible charts (alt-text) | design-spec | medium | low | document | pilot-008, styleguide-002 | Editorial, Technical |
| food-security-briefs-source-017 | Vet additional open sources (World Bank/OWID) — licence verification | research | small | medium | document | gate-004 | Licence |
| food-security-briefs-i18n-015 | Translate a delivered brief (domain + language reviewer) | translation | small | medium | translation | template-014, brief-011 | Domain expert, Editorial |
| food-security-briefs-brief-016 | Briefs + datasets #4–#5 across geographies/indicators | data | large | medium | dataset | template-014, partner-013 | Domain expert, Editorial, Licence |

**Acceptance criteria — key tasks**

- **template-014 (scalable brief template + accessible charts)**
  - [ ] Template encodes the style guide + statistical-integrity requirements as fill-in sections.
  - [ ] Charts generated deterministically with committed **alt-text** and accessible color/contrast;
        readability target enforced.
  - [ ] Reduces per-brief effort (measured against the M0/M1 baseline in the outcome ledger).

- **source-017 (vet additional open sources)**
  - [ ] Each candidate source's licence verified for derivatives with cited clause/URL + attribution
        terms recorded; non-commercial/non-derivative/unclear/acute-crisis feeds EXCLUDED.
  - [ ] A short admit/defer/exclude decision record committed per source.

- **i18n-015 (translate a delivered brief)**
  - [ ] Translation reviewed by a domain + language reviewer for accuracy and non-alarmist fidelity.
  - [ ] Attribution + non-endorsement + "not advice" preserved in the target language.

**M2 Definition of Done:** scalable template with accessible (alt-text) charts and readability target
met; ≥ 1 brief translated with domain + language review; ≥ 5 briefs + datasets delivered cumulatively;
≥ 1 additional source licence-verified or formally deferred; median per-brief effort reduced vs.
M0/M1 baseline.

---

## Milestone M3 — Outcomes & sustainability

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| food-security-briefs-reuse-018 | Track and verify external reuse/outcome events | research | small | low | document | brief-011, brief-012, brief-016 | Steward |
| food-security-briefs-refresh-019 | Refresh process tied to FAOSTAT release cycle + drift detection | maintenance | small | low | document | harness-006 | Maintainer |
| food-security-briefs-scale-020 | Briefs + datasets #6–#8 across ≥ 3 FAOSTAT domains | data | large | medium | dataset | brief-016, refresh-019 | Domain expert, Editorial, Licence |

**Acceptance criteria — key tasks**

- **reuse-018 (reuse/outcome tracking)**
  - [ ] ≥ 3 verifiable external reuse events recorded in the outcome ledger (citation, syllabus/lesson,
        republish, partner confirmation), each with externally verifiable evidence.

- **refresh-019 (refresh + drift detection)**
  - [ ] Detects when a FAOSTAT dataset has been re-released vs. the pinned version.
  - [ ] A re-release schedules a `maintenance` rebuild **and re-review** (revisions can change the
        story); stale briefs are flagged, never silently updated.

**M3 Definition of Done:** ≥ 3 verifiable reuse events; ≥ 8 briefs + datasets delivered cumulatively
across ≥ 3 FAOSTAT domains; refresh process tied to the FAOSTAT release cycle documented; steward
identified for ongoing liaison + outcome tracking.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| food-security-briefs-catalog-021 | Build TOPIC-CATALOG.md (indicator backlog, licence/sensitivity pre-triaged) | research | medium | low | document | Feeds per-topic tasks; each entry still needs gate-004 |
| food-security-briefs-dash-022 | Outcome dashboard (deliveries + reuse events, reads the ledger) | code | medium | low | pr | Supports success-metric tracking |
| food-security-briefs-validator-023 | Standalone Croissant + Frictionless validator CLI | code | small | low | pr | Lets any reuser re-validate a derivative dataset |
| food-security-briefs-i18n-024 | Translate brief set into an additional language | translation | medium | medium | translation | Needs domain + language reviewer; widens reach |
| food-security-briefs-region-025 | Regional-aggregate brief (e.g. continental food supply trends) | data | medium | medium | dataset | Aggregate-only; same three gates |

---

## Example task JSON

```json
{
  "id": "food-security-briefs-styleguide-002",
  "title": "Non-alarmist editorial style guide + \"not advice\" framing",
  "project": "food-security-briefs",
  "type": "writing",
  "lane": "donated",
  "priority": "high",
  "domain": ["food-security", "agriculture", "public-data", "open-knowledge"],
  "riskTier": "medium",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "small",
  "status": "open",
  "context": "food-security-briefs turns FAOSTAT (CC BY 4.0 IGO) and other openly-licensed food/agriculture statistics into derivative datasets and plain-language briefs. Food security is a sensitive civic topic where public coverage is frequently alarmist, decontextualized, or politically charged. Before any brief is written, the project needs a committed editorial standard that makes non-alarmism and 'this is information, not advice' enforceable rather than a matter of reviewer taste.",
  "objective": "Create the non-alarmist editorial style guide and 'not advice' framing that every brief must pass, including the reviewer checklist that produces a committed pass/flag artifact per brief.",
  "acceptanceCriteria": [
    "Bans sensational/emotive framing, causal/political attribution, prediction beyond the source, country-shaming or provocative rankings, and any dietary/medical/agricultural/financial/policy advice.",
    "Requires every brief to state the indicator definition, reference period, status flags, and published uncertainty, plus an explicit 'what the data does not say' section.",
    "Specifies the FAO attribution + non-endorsement disclaimer placement and the exact 'not advice' notice wording.",
    "Sets a published readability target (grade level / metric) and defines a reviewer checklist that yields a committed pass/flag artifact per brief; one unresolved flag blocks publication.",
    "Distinguishes chronic/structural indicators from acute-crisis concepts (out of scope) so they are never conflated.",
    "pnpm build && pnpm test && pnpm lint pass for any committed tooling; commit is DCO signed-off."
  ],
  "resources": [
    "C:\\code\\elyos\\planning\\projects\\food-security-briefs\\PLAN.md",
    "C:\\code\\elyos\\docs\\good-deed-definition.md",
    "FAOSTAT terms of use (CC BY 4.0 IGO)",
    "Creative Commons Attribution 4.0 International IGO"
  ],
  "output": "A committed non-alarmist editorial style guide + 'not advice' framing document and reviewer checklist, ready for use by every per-topic brief task.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```

---

## Task rollup

- **20 scheduled tasks** across M0–M3 (M0: 8 · M1: 5 · M2: 4 · M3: 3) + **5 backlog tasks** = 25 total.
- By type: research 6 · design-spec 4 · code 5 · writing 1 · data 8 · translation 2 · maintenance 1.
- By risk: `low` 10 · `medium` 15 · `high` 0 (any high-stakes acute-crisis/advice topic is excluded by
  the sensitivity screen, never created as a task).
- Every brief/dataset task passes three blocking gates (domain expert, editorial, licence) and the
  reproducible build harness; all start `verifiedNeed: false` until a named partner confirms use.

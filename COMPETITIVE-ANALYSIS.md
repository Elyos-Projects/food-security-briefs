# Competitive + Improvement Analysis — food-security-briefs

_Analysis date: 2026-06-29. Subject: Elyos donated-lane project producing open, plain-language briefs +
reproducible derivative datasets from authoritative open food/agriculture statistics (FAOSTAT-first),
risk tier medium. Plan reviewed: PLAN.md v0.1.0 (2026-06-28). Web-grounded where claims touch external
sources; URLs cited inline._

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually strong: 17 H2 sections, constraints-as-identity framing, three blocking review
gates (statistical / editorial / licence), deterministic reproducible builds, "every figure resolves to
a dataset row," and an honest `verifiedNeed: false` posture until a partner is named. The single highest
risk for this kind of project — conflating chronic structural indicators with acute-crisis
classification — is correctly and explicitly excluded (IPC / Cadre Harmonisé out of scope). That is the
right call and is well defended.

**Material finding 1 — the license identifier is wrong (load-bearing).** The plan asserts, repeatedly and
in the canonical metadata model (`license { id: "CC-BY-4.0-IGO" }`), that FAOSTAT data is **CC BY 4.0
IGO**. FAO's current Statistical Database Terms of Use state the data is licensed under the **standard
Creative Commons Attribution 4.0 International (CC BY 4.0)** — _not_ the IGO variant
(https://www.fao.org/contact-us/terms/db-terms-of-use/en/, verified 2026-06-29; the FAOSTAT "Suite of
Food Security Indicators" catalog entry likewise reads CC-BY-4.0:
https://data.apps.fao.org/catalog/dataset/faostat-food-security). FAO historically used CC BY 3.0 IGO,
which is likely the source of the error. Three concrete consequences: (a) the SPDX identifier
`CC-BY-4.0-IGO` **does not exist** on the SPDX license list (SPDX has `CC-BY-3.0-IGO` but no 4.0-IGO),
so a "SPDX identifiers for licences" gate would fail validation on the project's own canonical model;
(b) the prescribed disclaimer wording ("_This is an adaptation of an original work by FAO… not endorsed
by FAO_") is IGO-licence boilerplate, not what CC BY 4.0 requires; (c) the "IGO mediation/arbitration
clause" the plan documents is an IGO-licence feature that does not apply. The _substance_ survives — FAO
still requires attribution and still imposes a non-endorsement restriction in its terms ("you may not in
any way represent that FAO has… endorsed… your use") — so the gate's intent is correct, but the license
id, SPDX value, disclaimer template, and arbitration note all need correcting. Because the entire
licence gate, attribution string, and provenance snapshot are built around the IGO variant, this is the
most important fix.

**Material finding 2 — the in-scope indicator set under-uses FAOSTAT's own structural metrics and risks a
subtle chronic/acute slip.** The plan's pilot candidates (food supply kcal/capita/day, cereal production)
are well chosen, but the headline FAOSTAT food-security indicator the plan leans on — **Prevalence of
Undernourishment (PoU)** — carries heavier caveats than the plan currently names. PoU is a _modeled_
estimate derived from food-supply, distribution (inequality) and dietary-energy-requirement parameters,
published as a **three-year average** with an uncertainty interval, and FAO **back-revises the entire
historical series every report** (https://www.fao.org/americas/news/news-detail/sofi-2024/en;
methodology: https://openknowledge.fao.org/server/api/core/bitstreams/0556ea9c-65bb-46e9-aa6b-39fdeb8afbe7/content/sofi-statistics-rlc-2024/sdg-2-prevalence-undernourishment.html).
The statcheck checklist should explicitly require: (i) labelling PoU as modeled (not observed), (ii)
never comparing a current PoU point to a prior _report's_ PoU point (series is re-estimated), and (iii)
distinguishing PoU from the survey-based **FIES** (Food Insecurity Experience Scale, also a FAOSTAT
indicator) — the two answer different questions and are routinely conflated in coverage. The plan
mentions PoU bounds but not the back-revision trap or the PoU-vs-FIES distinction; both belong in the
checklist.

**Other correctness notes (smaller):**
- **Currency / re-release.** The refresh-as-re-review design (M3) is correct and important precisely
  because of the PoU back-revision behavior above. Good. Consider pinning not just dataset version but
  the SOFI/report edition a figure was sourced against.
- **Region-specificity.** ISO3 + UN M49 normalization is correct and addresses a real FAOSTAT pitfall
  (M49 "regions"/aggregates vs. countries; "China" includes/excludes Taiwan/HK/Macau variants in
  FAOSTAT). The plan should add an explicit rule that FAOSTAT _aggregate_ rows (regional/"World") are
  never silently mixed with country rows — the status-flag decoder covers "aggregate" but the brief
  guidance should too.
- **Non-partisan / non-alarmist.** Well covered; the explicit ban on "worst-offender" rankings and
  causal/political attribution is exactly right for this topic.
- **Document-not-original-research boundary.** Cleanly drawn ("no forecasting/modelling beyond the
  source"). One tension: PoU is _itself_ a model — so "we don't model" must mean "we don't add models,"
  and the brief must still explain that the source figure is modeled. Worth a sentence so it doesn't read
  as contradictory.
- **Attribution/licensing.** Beyond the IGO error, the multi-source plan (World Bank, OWID, UN WPP) is
  correctly gated behind per-source verification; OWID is indeed CC BY (confirmed below), World Bank Open
  Data CC BY 4.0 — those are right.
- **Completeness.** No gaps in required sections. The plan honestly flags its biggest weakness (no
  secured partner) rather than papering over it.

---

## 2. Competitive landscape (web-grounded)

The "competitors" are mostly the authoritative _sources_ the project sits atop — the real competition is
for the **explainer / interpretation layer**, where OWID is the incumbent.

- **FAO SOFI (State of Food Security & Nutrition in the World)** — the annual flagship; authoritative,
  peer-reviewed, the definitive PoU/FIES numbers. _Strength:_ ultimate authority + methodology depth.
  _Weakness:_ a 200+ page annual PDF, technical, slow cadence, not a per-topic plain-language unit; uses
  three-year averages and back-revises silently for lay readers.
  https://www.fao.org/publications/fao-flagship-publications/the-state-of-food-security-and-nutrition-in-the-world/en
- **IPC (Integrated Food Security Phase Classification)** — the 5-phase acute-food-insecurity / famine
  standard (Phase 1 Minimal → 5 Catastrophe/Famine; ≥20% of an area's population threshold). _Strength:_
  the global benchmark for _acute_ crisis severity. _Weakness:_ acute, fast-moving, alarmism-prone,
  consensus-driven and infrequent — and explicitly **out of scope** for this project (correctly).
  https://www.ipcinfo.org/ipcinfo-website/ipc-overview-and-classification-system/ipc-acute-food-insecurity-classification/en/
- **FEWS NET (USAID-funded)** — scenario-based food-security _outlooks_ for ~30 countries; IPC-compatible
  early warning. _Strength:_ forward-looking, analyst-rich, remote-sensing + markets. _Weakness:_
  forecasting (out of scope here), country-limited, US-government-funded (subject to funding/political
  risk). https://fews.net/about/integrated-phase-classification
- **USDA ERS — Food Security in the U.S.** — the gold standard for _household_ food-insecurity
  measurement (CPS-FSS survey, the 18-item HFSSM module). _Strength:_ rigorous survey methodology,
  authoritative for the US. _Weakness:_ **US-only**, survey-microdata-based (the project hard-excludes
  microdata), annual.
  https://www.ers.usda.gov/topics/food-nutrition-assistance/food-security-in-the-us/measurement
- **WFP HungerMap LIVE** — near-real-time monitoring of 90+ countries with ML "nowcasts" and 30–90 day
  predictions, fusing IPC, live phone-survey calls, market, climate and hazard data. _Strength:_
  real-time, granular, visually compelling. _Weakness:_ predictive/modeled (out of scope here), operational
  rather than explanatory, less about teaching the reader what the numbers mean.
  https://hungermap.wfp.org/ ; https://innovation.wfp.org/project/hungermap-live
- **Our World in Data (OWID) — Hunger & food** — _the direct competitor for the explainer layer._
  Plain-language articles ("How is food insecurity measured?", "Hunger and Undernourishment"), tidy
  re-published datasets, all **CC BY**. _Strength:_ huge reach, trusted, genuinely readable, already
  does FAO-derived charts + standardized country names. _Weakness:_ broad (food is one of dozens of
  topics, not a dedicated focus), not partner-delivered to specific beneficiaries, doesn't ship a
  formal reproducible Frictionless+Croissant data package per topic, and provenance is lighter than this
  project's proposed standard. https://ourworldindata.org/food-insecurity ;
  https://ourworldindata.org/hunger-and-undernourishment
- **Global Hunger Index (GHI)** — annual composite (PoU + child stunting + wasting + under-5 mortality),
  now published by Concern Worldwide + Welthungerhilfe (IFPRI handed it over in 2018). _Strength:_ single
  comparable score, strong policy communication. _Weakness:_ composite indices invite dispute — India's
  government publicly contested the 2024 methodology (outdated NFHS-4 data, modeled PoU, child-metric
  overweighting), illustrating exactly the politicization/alarmism trap this project avoids.
  https://www.globalhungerindex.org/methodology.html ; https://en.wikipedia.org/wiki/Global_Hunger_Index
- **IFPRI** — research institute (CGIAR); deep food-policy analysis and modeling. _Strength:_ rigor,
  credibility. _Weakness:_ academic register, not plain-language per-topic briefs for advocates/journalists.
  https://www.ifpri.org/

**Takeaway:** none of these combine (plain-language explainer) + (per-topic reproducible derivative
dataset with full provenance) + (last-mile delivery to a named beneficiary) + (a hard non-alarmist,
chronic-only, no-advice editorial standard). OWID is closest but is broad, non-delivered, and lighter on
formal provenance.

---

## 3. Gaps we can fill

1. **The "what it does NOT say" layer.** Every source publishes numbers; almost none teach lay readers
   the caveats (modeled vs observed, 3-yr averages, back-revision, PoU≠FIES, aggregate≠country). This is
   the project's natural moat.
2. **Reproducible derivative data packages per topic.** OWID re-publishes data but not as a formally
   validatable Frictionless Tabular Data Package + Croissant ML bundle with a committed build script and
   checksum that a third party can re-derive. That's a genuine, fillable gap for the open-data reuse
   community.
3. **Pre-cleared licence + attribution.** Reusers routinely strip FAO attribution/non-endorsement (and
   often cite the wrong CC variant — as this very plan did). Shipping the verified license, correct
   attribution string, and snapshot _with_ the data removes a real adoption barrier.
4. **A calm baseline for fact-checkers.** Food/hunger claims are a misinformation magnet; a sourced,
   non-alarmist, checkable baseline brief is something neither SOFI (too long) nor news (too hot) provides.
5. **Plain-language structural indicators for advocates/journalists** who currently overreach into acute
   IPC framing because the structural FAOSTAT story is harder to read.
6. **Accessibility + readability** (alt-texted charts, grade 8–9 prose, translations) — under-served
   across all the authoritative sources, which skew technical.

---

## 4. Differentiators to win

- **Trust-as-product, made auditable.** Every figure resolves to a committed dataset row; every dataset
  re-derives from source in CI. This is _falsifiability as a feature_ — stronger than any competitor's
  provenance.
- **The refusals are the brand.** Hard, blocking exclusions (no forecasting, no acute-crisis, no advice,
  no rankings, no causal attribution) are exactly the credibility signals advocates/journalists/libraries
  need — and the opposite of the alarmist coverage they're trying to escape.
- **Three independent blocking gates** (statistical, editorial, licence) so a brief can't ship
  correct-but-alarmist or readable-but-wrong.
- **Delivered, not published.** Outcome = used by a named beneficiary, not pageviews — distinct from
  OWID's broadcast model and aligned to Elyos's "delivered, not merged."
- **Reproducible, interoperable data bundles** (Frictionless + Croissant) that downstream reusers and ML
  pipelines can consume directly, with the licence already cleared.

---

## 5. Claude API leverage — and the hard limits

**Where Claude adds real leverage (drafting / scaling / explaining):**
1. **First-draft plain-language briefs from the derivative dataset only.** Feed Claude the tidy dataset +
   canonical metadata (indicator definition, reference period, decoded flags, uncertainty bounds) and have
   it draft the explainer + the "what this does not say" section + alt-text, constrained so every number it
   uses is passed in (never recalled). Pairs perfectly with "every figure resolves to a dataset row."
2. **Statistical-caveat explanation at grade 8–9.** Claude is strong at turning "PoU is a 3-year moving
   average of a modeled estimate with a published uncertainty interval, back-revised each release" into
   calm, correct lay prose — and at drafting the statcheck/style-guide _checklists_ themselves.
3. **Region-scaling + translation drafts.** Re-instantiate one reviewed brief template across
   indicators/geographies and produce first-pass translations (then domain+language human review). Also:
   automated _pre-review linting_ — Claude as a non-blocking "alarmism/advice/attribution" flagger that
   surfaces candidate issues for the human editorial gate.
4. **Provenance + license-string assembly** and an MCP-server "data-brief engine" (see §7).

**Where Claude must NOT decide (must remain human/deterministic):**
- **Data accuracy & the numbers themselves.** Figures come from the deterministic build harness, never
  generated or "remembered" by Claude. No fabricated/estimated figures — ever. A figure not in the
  committed dataset cannot appear in a brief.
- **License verification.** Whether a source permits derivatives, and the exact license id / attribution /
  non-endorsement text, is a human licence-gate decision against the actual terms page (this analysis is
  itself proof: the IGO error shows why a model recalling "FAO = CC BY IGO" must not be authoritative).
- **Non-partisan / non-alarmist sign-off** and **statistical-caveat correctness** — the domain-expert and
  editorial human gates remain blocking; Claude only proposes, never approves.
- **Sensitivity / scope screen** (chronic vs acute, advice-adjacent) — escalation/exclusion is a human call.

---

## 6. Ten concrete optimizations

1. **Fix the licence to CC BY 4.0 (drop "IGO").** Correct the canonical model `license.id`, the SPDX
   value (use `CC-BY-4.0`; `CC-BY-4.0-IGO` is not a valid SPDX id), the attribution string, the
   non-endorsement disclaimer wording, and remove the IGO mediation/arbitration note. (See §1.)
2. **Upgrade the statcheck checklist for PoU specifically:** label modeled-not-observed; ban cross-_report_
   PoU comparisons (series is back-revised); require the PoU-vs-FIES distinction whenever either appears.
3. **Add an "aggregate vs country" guard** to the brief style guide and validator (never mix M49 aggregate
   rows with country rows; flag "World"/region rows explicitly).
4. **Pin the source _edition_, not just dataset version** (e.g., "FAOSTAT release X, as cited in SOFI 2025")
   so back-revisions are auditable and the refresh-as-re-review trigger is precise.
5. **Add FIES as an early in-scope indicator** alongside PoU — it's survey-based, structural, FAOSTAT-native,
   and lets briefs _contrast_ the two measures, which is a teaching differentiator (still aggregate-only).
6. **Ship a one-page "how to read this number" companion** auto-generated per indicator (definition, what
   it is/isn't, common misreads) — a reusable artifact that scales the moat.
7. **Add a deterministic "claim-to-row" linter** in CI: parse every numeral in a brief and assert it
   matches a committed dataset cell within tolerance — operationalizes the "every figure resolves to a row"
   rule instead of relying on reviewer diligence.
8. **Pre-write the alarmism/advice lint as machine checks** (banned-phrase list: "time bomb," "crisis,"
   "starvation," superlatives, future-tense predictions) as a non-blocking pre-screen feeding the editorial
   gate.
9. **Make the self-serve fallback first-class now** (Zenodo DOI + open repo + landing page) so M0 yields a
   real delivered+citable outcome regardless of partner timing — and so each brief is itself externally
   citable (helps the ≥3 reuse-events metric).
10. **Define the reconciliation tolerance numerically** (an open question in the plan) before the harness
    is built — e.g., exact match on integer counts, ≤0.1pp on PoU given rounding — so "reproduces source
    totals" is testable in CI from day one.

---

## 7. Parallel & perpendicular spin-offs

- **Reusable data-brief engine (the strongest spin-off).** The build harness + canonical metadata model +
  Frictionless/Croissant emitters + brief template + three-gate checklist are **source-agnostic**. Generalize
  them into an Elyos `data-brief-kit` any open-data-explainer project can instantiate (World Bank, WHO,
  Eurostat, UNESCO, energy/climate data). This is the platform play.
- **MCP server: "authoritative-data-brief" tools.** Expose `fetch_indicator`, `decode_flags`,
  `build_data_package`, `lint_claims_to_rows`, and `assemble_attribution` as MCP tools so any Claude/agent
  workflow can draft a sourced, license-clean brief without ever inventing a number — the deterministic
  data path stays outside the model.
- **Parallel siblings (same engine, adjacent domains):**
  - _open-data-explainers_ — the generic plain-language-explainer-from-open-data line this is a member of.
  - _incidence-explorers_ — disease/health-incidence explainers (WHO/IHME) reuse the identical
    chronic-vs-acute, modeled-vs-observed, uncertainty-preserving discipline.
  - _food-assistance-maps_ — geography-aware delivery/visualization layer (carefully kept on the
    structural/non-acute side; would consume the same datasets).
  - _open-gardening-guides_ — community-facing companion (practical, complementary to the data briefs;
    keep the "no advice" wall between them clear).
- **Perpendicular:** a public **"how to read a statistic" curriculum** (modeled vs observed, moving
  averages, back-revision, composite-index pitfalls) — directly serves the educator beneficiary and is
  marketable to journalism-training orgs.

---

## 8. Open questions

1. **License correction scope:** beyond fixing the id, does any already-drafted attribution/snapshot need
   re-issuing? (Confirm the exact CC BY 4.0 attribution string and non-endorsement sentence FAO's terms
   require — see https://www.fao.org/contact-us/terms/db-terms-of-use/en/.)
2. **PoU vs FIES positioning:** lead with FIES (survey-based, arguably more intuitive) or PoU (the headline
   SDG 2.1.1 number) for the pilot?
3. **Reconciliation tolerance:** what numeric tolerance counts as "reproduces source totals" given FAOSTAT
   rounding/imputation? (Plan's own open question — block the harness on it.)
4. **First beneficiary type + channel** — educators vs newsrooms vs libraries vs civil-society — and is the
   Zenodo self-serve route acceptable as a _delivered_ outcome to the steward?
5. **Acute-indicator exception:** keep the blanket IPC/acute exclusion, or ever admit one well-licensed
   acute indicator under escalated expert review? (Plan defaults to blanket exclusion — recommend keeping it.)
6. **Where Claude sits in the pipeline:** sanctioned as a drafting/linting assistant with a written "Claude
   may propose, humans approve; numbers never come from Claude" policy — should that policy be a committed
   M0 artifact alongside the style guide?

---

### Sources
- FAO Statistical Database Terms of Use (license = CC BY 4.0, non-endorsement): https://www.fao.org/contact-us/terms/db-terms-of-use/en/
- FAOSTAT Suite of Food Security Indicators (CC-BY-4.0): https://data.apps.fao.org/catalog/dataset/faostat-food-security
- FAO SOFI flagship: https://www.fao.org/publications/fao-flagship-publications/the-state-of-food-security-and-nutrition-in-the-world/en
- PoU methodology / 3-yr average / annual back-revision: https://www.fao.org/americas/news/news-detail/sofi-2024/en ; https://openknowledge.fao.org/server/api/core/bitstreams/0556ea9c-65bb-46e9-aa6b-39fdeb8afbe7/content/sofi-statistics-rlc-2024/sdg-2-prevalence-undernourishment.html
- IPC acute classification (5 phases, 20% threshold): https://www.ipcinfo.org/ipcinfo-website/ipc-overview-and-classification-system/ipc-acute-food-insecurity-classification/en/
- FEWS NET / IPC-compatibility: https://fews.net/about/integrated-phase-classification
- USDA ERS food-security measurement (CPS-FSS / HFSSM): https://www.ers.usda.gov/topics/food-nutrition-assistance/food-security-in-the-us/measurement
- WFP HungerMap LIVE: https://hungermap.wfp.org/ ; https://innovation.wfp.org/project/hungermap-live
- OWID hunger/food (CC BY; FIES Rasch model): https://ourworldindata.org/food-insecurity ; https://ourworldindata.org/hunger-and-undernourishment
- Global Hunger Index methodology + dispute: https://www.globalhungerindex.org/methodology.html ; https://en.wikipedia.org/wiki/Global_Hunger_Index
- IFPRI: https://www.ifpri.org/

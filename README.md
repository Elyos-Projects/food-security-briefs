# food-security-briefs

> The Food and Agriculture Organization of the United Nations (FAO) publishes FAOSTAT, one of the largest open statistical corpora on food, agriculture, nutrition, trade, and food security — over 245 co  ·  **Risk tier:** med  ·  **Status:** planning

The Food and Agriculture Organization of the United Nations (FAO) publishes FAOSTAT, one of the largest open statistical corpora on food, agriculture, nutrition, trade, and food security — over 245 countries/territories and 20,000+ indicators, licensed **CC BY 4.0 IGO**. The data is authoritative but hard to use: indicator definitions are technical, values carry status flags (official / estimated / imputed / aggregate), food-security headline indicators are published as multi-year moving averages with uncertainty ranges, country aggregates and reference periods are easy to misread, and the licence carries an intergovernmental-organization (IGO) attribution + **non-endorsement** obligation that most reusers ignore. Meanwhile, public-facing coverage of hunger and food security is frequently alarmist, decontextualized, or politically charged.

**Definition of shipped:** gate (CC-BY-IGO, attribution + non-endorsement, aggregate-only, no third-party-data entanglement); (2) passed the statistical-integrity checklist and **domain expert** sign-off; (3) passed the **editorial non-alarmist** review; (4) is **reproducible** (build re-derives from sourc

This is an **Elyos** good-deed project. Contributors pull a task, do it with their own coding agent, and open a PR. Platform: https://github.com/jdev1977/elyos

## Plan
- [PLAN.md](./PLAN.md) — robust enterprise plan (vision, architecture, roadmap, risks; includes an applied-improvements appendix + review sign-off)
- [TASKS.md](./TASKS.md) — schema-mapped task backlog
- [tasks/](./tasks/) — ready-to-pull task JSON(s)

## Contribute
```bash
elyos browse
elyos next --repo Elyos-Projects/food-security-briefs --no-fork
```

## Licensing & review
- Open license (see PLAN.md).
- Risk tier **med** — deeds are *delivered, not merged*; a domain reviewer (and expert sign-off for any high-stakes content) must approve before merge.

> Planning stage; no adopting partner secured yet (`verifiedNeed: false` on delivery-dependent tasks).

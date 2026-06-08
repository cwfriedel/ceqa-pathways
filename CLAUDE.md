# CEQA Decision Pathways — Working Memory

## What this is

A single-page web reference, **"CEQA Decision Pathways,"** mapping every available CEQA pathway for forest health & fuels reduction projects in California. Built by **Chris Friedel** as the CEQA exemptions deliverable for the **Permitting & Environmental Compliance subcommittee** of the Nevada County **Quarterly Wildfire Stakeholders Group (QWSG)**. It is the digital form of the subcommittee's CEQA flowchart product.

**Work in progress — updated periodically.** Framed as a working summary, **not legal advice**.

- **Live site:** https://ceqa-pathways.vercel.app (Vercel; the static HTML also deploys to Netlify Drop / GitHub Pages)
- **GitHub:** cwfriedel/ceqa-pathways
- **Current as of:** May 2026 — updated for the 2025 legislative session (SB 131, AB 404, March 2025 Emergency Proclamation)

This subcommittee deliverable feeds the larger QWSG effort. For full collaborative context (people, governance, portfolio process, related work), see the QWSG project memory in the Yuba Watershed Institute shared drive (`collaborations/QWSG/CLAUDE.md`).

## Architecture

One self-contained, **hand-written `index.html`** — no build step, framework, or package manager. The only external dependency is Google Fonts (Newsreader, IBM Plex Sans, IBM Plex Mono).

- `index.html` — the live site (entry point)
- `CEQA Pathways Reference-print.html` — print-optimized variant: strips interactive chrome, renders the decision tree as a static flowchart, prints full URLs alongside link text, and auto-opens the print dialog. Used to produce a US-Letter PDF handout for QWSG meeting packets.

## Three main parts

1. **Interactive decision tree** — 5 Yes/No questions that recommend a pathway:
   commercial timber/biomass sale? → is it a CEQA "project"? → federal land with NEPA complete? → fits a statutory/categorical exemption? → within a Program EIR scope?
2. **Exemption menu** — 8 pathway cards (eligibility, triggers, common uses, statute links).
3. **Working notes** (locally framed practitioner guidance) + **Resources** (outbound links: PRC, CEQA Guidelines, 2025 bill text, CalVTP PEIR, CDFW SERP, FHSZ maps, CEQAnet).

## The 8 pathways

1. **AB 404 / PRC §4799.05(d)** — NEPA-reviewed federal (BLM/USFS) projects. Statutory; sunsets Jan 1 2028.
2. **SB 131 / PRC §21080.49** — wildfire risk reduction, 4 categories, urban-interface-focused. Statutory; no sunset; new in 2025.
3. **SERP / PRC §21080.56** ("Cutting the Green Tape") — habitat restoration, no acreage cap. Statutory; sunsets Jan 1 2030.
4. **Class 4 / §15304** — Minor Alterations to Land; the workhorse for small fuels/forestry. Categorical; §15300.2 exceptions apply.
5. **Other categorical** — Classes 1, 7, 8, 33.
6. **March 2025 Emergency Proclamation** (EO N-18-25 / N-38-25) — closed to new apps May 1 2026; historic record only (approved projects remain valid).
7. **Special / narrow-use** — oak woodland, declared emergency, planning studies.
8. **Forest Practice Act exemptions / 14 CCR §1038** — CEQA-equivalent for commercial timber.

## Editing

Content is hand-edited directly in `index.html` — no templating or markdown source:

| What to change | Where |
|---|---|
| "Current as of" date | hero eyebrow (top of `<body>`) + footer colophon |
| "What changed recently" cards | `<div class="context-grid">` near the top |
| A decision-tree question or recommendation | `STEPS` and `RESULTS` objects in the `<script>` at the bottom |
| An exemption card | `<article class="ex">` blocks in the `#exemptions` grid |
| A working note | `<div class="notes-list">` in `#notes` |
| Outbound link URLs | `#resources` block + `.ex-foot` links in each card |

Preview by opening `index.html` in a browser. **Commit and push to redeploy.**

## Contributing

Updates from QWSG members and partner agencies welcome: open a GitHub issue → submit a PR → **subcommittee review before merge** (it's a public-facing reference, so review is intentional). Flag non-trivial changes (new pathway, restructured tree, major reframing) at the next subcommittee meeting before opening the PR.

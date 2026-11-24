# CEQA Decision Pathways — Working Memory

## What this is

A single-page web reference, **"CEQA Decision Pathways,"** mapping every available CEQA pathway for forest health & fuels reduction projects in California. Built by **Chris Friedel** as the CEQA exemptions deliverable for the **Permitting & Environmental Compliance subcommittee** of the Nevada County **Quarterly Wildfire Stakeholders Group (QWSG)**. It is the digital form of the subcommittee's CEQA flowchart product.

**Work in progress — updated periodically.** Framed as a working summary, **not legal advice**.

- **Live site:** https://ceqa-pathways.vercel.app (Vercel project `ceqa-pathways`), proxied by the YWI website at `https://www.yubawatershedinstitute.org/ceqa-pathways/index.html` and embedded at `/resources/ceqa-decision-pathways`
- **GitHub:** cwfriedel/ceqa-pathways (**public**, and the **source of truth**). The YWI site repo (`cwfriedel/ywi-nextjs`, private) holds no copy; its `next.config.js` rewrites `/ceqa-pathways/*` to this deployment. Edit here, never there.
- **Current as of:** October 2026 — updated through the 2025–26 legislative session (SB 131, March 2025 Emergency Proclamation and its successor SB 1370, AB 1699, AB 442)

This subcommittee deliverable feeds the larger QWSG effort. For full collaborative context (people, governance, portfolio process, related work), see the QWSG project memory in the Yuba Watershed Institute shared drive (`collaborations/QWSG/CLAUDE.md`).

## Architecture

One self-contained, **hand-written `index.html`** — no build step, framework, or package manager, and no external dependencies. Fonts (Newsreader, IBM Plex Sans, IBM Plex Mono) are self-hosted latin-subset woff2 files in `fonts/`, declared with `@font-face` at the top of the page's `<style>`.

- `index.html` — the live site (entry point). Printing is handled by its own print CSS plus a small `beforeprint` script that expands the "Read more" panels; keep it.
- `fonts/` — self-hosted fonts. Keep them self-hosted: the YWI site CSP is `font-src 'self'`, so a Google Fonts `<link>` would break on the proxied page.
- `docs/pathway-finder-redesign.md` — design notes and the rule matrix for the pathway finder.
- `LICENSE` (MIT, code) + `LICENSE-CONTENT.md` (CC BY 4.0, content); `CONTRIBUTING.md`; `.github/ISSUE_TEMPLATE/`.

Things in `index.html` that exist for the hosting setup, don't remove them:
- `<link rel="canonical">` pointing at the YWI URL (avoids duplicate-content competition with ceqa-pathways.vercel.app).
- The draft-banner script at the end of `<body>`: shows a "Draft for subcommittee review" bar on any `*.vercel.app` host other than `ceqa-pathways.vercel.app`, i.e. on preview deployments.
- Root-relative `/_vercel/insights/script.js` (Vercel Analytics). Keep other asset URLs **relative** (`fonts/…`, `favicon.svg`) so they resolve under the YWI `/ceqa-pathways/` proxy path.

## Three main parts

1. **Pathway finder** — a project-profile questionnaire (who approves/funds the work, land ownership, NEPA status, planned activities, footprint, distance to structures, tree size, community setting, FHSZ, sensitive resources, restoration purpose, program context, start date, and stand details for commercial harvest). Questions reveal as they become relevant. The right-hand panel scores every pathway as *Likely fit* / *Possible — confirm* / *Unlikely*, lists the findings behind each score, and can copy a summary for a memo to file.
2. **Exemption menu** — 8 pathway cards (eligibility, triggers, common uses, statute links).
3. **Working notes** (locally framed practitioner guidance) + **Resources** (outbound links: PRC, CEQA Guidelines, 2025 bill text, CalVTP PEIR, CDFW SERP, FHSZ maps, CEQAnet).

## The 8 pathways

1. **PRC §4799.05(d)** (SB 901, 2018; extended by AB 211, 2022) — NEPA-reviewed federal (BLM/USFS) projects. Statutory; sunsets Jan 1 2028. (AB 404 (Sanchez, 2025) failed; never describe it as operative.)
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
| "Current as of" date | three places: hero eyebrow, `hero-meta` Status field, footer colophon |
| "What changed recently" cards | `<div class="ctx">` blocks in `.context-grid` (at most one `ctx ctx-feature`). Each has a short `.ctx-sum` plus full text in `<details class="ctx-more">`. The spelled-out count in "*…* updates that reshape the menu" must match the number of cards. |
| A pathway-finder question, or a pathway's fit rules | `QUESTIONS[]` and `PATHWAYS[]` in the inline `<script>` at the bottom. Findings are `ok` / `chk` / `no` / `doc`; only the first three change the fit tier (`doc` is follow-through). Each pathway's `card` must point at its `#ex-N` exemption card. Sunset years appear in the `timing` question, the pathway `test` functions, `shelfLife()` and the card: move them together. Run `node --check` on the script afterwards, then click through a full profile in a browser. |
| An exemption card | `<article class="ex">` blocks in the `#exemptions` grid |
| A working note | `<div class="notes-list">` in `#notes` |
| Outbound link URLs | `#resources` block + `.ex-foot` links in each card |

Preview by opening `index.html` in a browser. Never push to `main` directly: branch, open a PR, and stop. Merging to `main` publishes to the live site and the YWI website at once.

## Contributing and review

Public repo: anyone can open an issue (templates in `.github/ISSUE_TEMPLATE/`) or a PR. **Subcommittee
review before merge.** Subcommittee members don't use GitHub, so review happens on the PR's Vercel
**preview link** (draft banner shown), which the maintainer emails to them; the maintainer records
the sign-off on the PR and merges. Vercel Authentication is turned off for this project so preview
links open without a Vercel login. See `CONTRIBUTING.md`. Flag non-trivial changes (new pathway,
restructured pathway finder, major reframing) at the next subcommittee meeting before opening the PR.

**Accuracy guardrails** (errors that have been fixed before, don't reintroduce them):
- The federal-NEPA CEQA exemption is PRC §4799.05(d), created by SB 901 (2018) and extended to a
  Jan 1, 2028 sunset by AB 211 (2022). AB 404 (Sanchez, 2025) failed; never describe it as operative.
- Distinguish enacted law from pending bills ("pending, not yet law"). Prefer "no recorded action
  since <date>" over asserting a bill is dead. Verify every status against a primary source.
- Keep the "not legal advice" framing (footer disclaimer + the "verify at the source" line in `#resources`).

**Maintenance:** a monthly scheduled task ("Ceqa pathways monthly legislation scan") scans for new CA
CEQA forestry/fuels legislation and drafts edits against this repo for review.

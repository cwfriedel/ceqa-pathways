# CEQA Decision Pathways

**A working reference for forest health & fuels reduction projects in California.**

Prepared by the Permitting & Environmental Compliance Subcommittee of the **Nevada County Quarterly Wildfire Stakeholders Group (QWSG)**.

🌐 **Live site:** https://www.yubawatershedinstitute.org/resources/ceqa-decision-pathways (also at https://ceqa-pathways.vercel.app)
📅 **Current as of:** October 2026

---

## What this is

A single-page web reference that maps every available CEQA pathway for forest health and fuels reduction projects — streamlined exemptions, programmatic tiers, Forest Practice documents, and the standard Initial Study → ND/MND/EIR route. It's updated through the 2025–26 legislative session, including **SB 131**, the **March 2025 Emergency Proclamation** framework and its 2026 statutory successor **SB 1370**, and the June 2026 draft state Wildfire and Landscape Resilience Action Plan.

The page replaces a dense two-page PDF reference with something easier to read, easier to share, and easier to navigate:

- **Pathway finder** — describe your project (who approves or funds it, land, activities, acreage, setting, resources, timing) and the right-hand panel scores each pathway as a likely, possible, or unlikely fit, with the reasons, and can copy a summary for a memo to file
- **Exemption menu** — the eight statutory, categorical, programmatic, and special-purpose pathways as full cards with eligibility, triggers, common uses, and links to the underlying statute
- **Working notes** — practitioner-level guidance on how each pathway maps to the kinds of projects QWSG participants are actually running
- **Resources** — direct outbound links to the Public Resources Code, CEQA Guidelines, 2025 bill text, CalVTP PEIR, CDFW SERP program, FHSZ maps, and CEQAnet

> ⚠️ This is a working summary, **not legal advice**. Statutes and guidelines change. Verify the current text and consult counsel before relying on any pathway for a specific project.

## Who it's for

- Participants in the Nevada County QWSG and its working subcommittees
- Resource Conservation Districts, fire safe councils, fire protection districts, CAL FIRE staff, BLM/USFS partners, and tribal natural resource programs operating in the Yuba and Bear river watersheds
- Private landowners, RPFs, and consultants working on WUI fuel reduction
- The general public — anyone trying to understand which CEQA path a given project should take

If you're outside Nevada County, most of the content still applies statewide; only the "Working notes" section is locally framed.

## Repository contents

```
.
├── README.md                              ← you are here
├── index.html                             ← the live site (entry point)
├── fonts/                                 ← self-hosted woff2 fonts
├── docs/                                  ← design notes (pathway-finder rule matrix)
├── CONTRIBUTING.md                        ← how to suggest changes; review process
└── LICENSE, LICENSE-CONTENT.md            ← MIT (code) + CC BY 4.0 (content)
```

The site is one self-contained HTML file plus self-hosted fonts (Newsreader, IBM Plex Sans, IBM Plex Mono) in `fonts/`. No external dependencies, no build step, no framework, no package manager.

## Running locally

```bash
git clone https://github.com/cwfriedel/ceqa-pathways.git
cd ceqa-pathways
# open the file in your browser — no server required
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Or, if you want a local server (handy for testing relative links):

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## How it's published

This repo is the **source of truth**. It is connected to a Vercel project:

- Every merge to `main` deploys to <https://ceqa-pathways.vercel.app>.
- The Yuba Watershed Institute website proxies that deployment at
  `yubawatershedinstitute.org/ceqa-pathways/`, and embeds it on its Resources page, so a merge here
  updates the YWI site too. Nothing needs to be copied into the YWI repo.
- Every pull request gets its own Vercel **preview link**, which shows a "draft for
  subcommittee review" banner. That link is what reviewers look at (see `CONTRIBUTING.md`).

Because the page is plain static HTML, you can also host a copy anywhere else (GitHub Pages,
Netlify Drop, any web server) without a build step.

## Printing

Print straight from the browser (**Save as PDF**, US Letter). The page's print styles hide the
interactive chrome and expand every "What changed recently" card so the full text prints.

## Updating the content

The HTML is hand-written and structured for direct editing — no templating, no markdown source.

| What you want to change | Where to edit |
|---|---|
| The "Current as of" date | Three places: hero eyebrow, `hero-meta` Status field, and footer colophon |
| The "What changed recently" cards | The `<div class="ctx">` blocks in `.context-grid` (keep the "*…* updates that reshape the menu" count in step) |
| A pathway-finder question or a pathway's fit rules | The `QUESTIONS[]` and `PATHWAYS[]` arrays inside the `<script>` at the bottom |
| An exemption card | The `<div class="exemptions">` grid; each card is an `<article class="ex">` |
| A working note | The `<div class="notes-list">` inside the `#notes` section |
| Outbound link URLs | The `<section id="resources">` block, and the various `.ex-foot` links inside each card |

Preview by opening `index.html` in a browser. Changes go live when a pull request is merged to `main`.

## Contributing

Corrections and suggestions from QWSG members, partner agencies, and the public are welcome:
[open an issue](https://github.com/cwfriedel/ceqa-pathways/issues/new/choose), or propose an edit
in a pull request. Every content change is reviewed by the Permitting & Environmental Compliance
Subcommittee before it is merged. See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the details.

## Credits & license

Maintained by the Permitting & Environmental Compliance Subcommittee of the Nevada County Quarterly
Wildfire Stakeholders Group, with the Yuba Watershed Institute.

- **Content** (text, pathway descriptions, fit rules, notes, `docs/`): [CC BY 4.0](LICENSE-CONTENT.md).
  Reuse and adapt it freely, including for other regions, with attribution and a link back here.
- **Code** (HTML, CSS, JavaScript): [MIT](LICENSE).
- **Fonts** in `fonts/`: SIL Open Font License 1.1, from their authors.

## Contact

For questions about the content, statute interpretation, or how a pathway applies to a specific project, contact the Permitting & Environmental Compliance Subcommittee through the Nevada County QWSG.

For website issues (broken links, typos, display problems), please open a GitHub issue.

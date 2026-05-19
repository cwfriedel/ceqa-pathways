# CEQA Decision Pathways

**A working reference for forest health & fuels reduction projects in California.**

Prepared by the Environmental Compliance Subcommittee of the **Nevada County Quarterly Wildfire Stakeholders Group (QWSG)**.

🌐 **Live site:** https://ceqa-pathways-chris-friedels-projects.vercel.app
📄 **Printable PDF:** see [Print handout](#print--pdf-handout) below
📅 **Current as of:** May 2026

---

## What this is

A single-page web reference that maps every available CEQA pathway for forest health and fuels reduction projects — streamlined exemptions, programmatic tiers, Forest Practice documents, and the standard Initial Study → ND/MND/EIR route. It's updated for the 2025 legislative session, including **SB 131**, **AB 404**, and the **March 2025 Emergency Proclamation** framework.

The page replaces a dense two-page PDF reference with something easier to read, easier to share, and easier to navigate:

- **Interactive decision tree** — answer five Yes/No questions and the right-hand panel recommends a pathway
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
├── CEQA Pathways Reference-print.html     ← print-optimized variant (auto-opens print dialog)
└── ...
```

The site is one self-contained HTML file. The only external dependency is Google Fonts (Newsreader, IBM Plex Sans, IBM Plex Mono). No build step, no framework, no package manager.

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

## Deploying

The site is static HTML — host it anywhere. Three easy options:

### Netlify Drop (fastest — under a minute)

1. Go to <https://app.netlify.com/drop>
2. Drag the repo folder onto the upload zone
3. Get a public URL immediately. Sign in to make it permanent and add a custom subdomain.

### GitHub Pages (free, lives next to the repo)

1. Push the repo to GitHub
2. **Settings → Pages → Source: `main` branch / `/` root → Save**
3. The site publishes at `https://<your-org>.github.io/ceqa-pathways/`
4. To use a custom domain (e.g. `ceqa.qwsg.org`), add a `CNAME` file containing the domain and configure DNS at your registrar.

### Vercel

1. Go to <https://vercel.com/new>
2. Import this repo (or drag the folder)
3. Vercel auto-detects the static site, builds nothing, and deploys

All three options give you HTTPS automatically and redeploy on every push.

## Print / PDF handout

`CEQA Pathways Reference-print.html` contains the same content with print-specific CSS that strips the interactive chrome, lays the decision tree out as a static flowchart, and prints full URLs alongside link text in the Resources section so the PDF works offline.

It also auto-opens the print dialog when loaded. To produce a PDF handout:

1. Open the print file in Chrome or Safari
2. The print dialog will appear on its own
3. **Destination: Save as PDF**, paper size **US Letter**, margins **Default**, scale **Default**

The result is a multi-page handout suitable for QWSG meeting packets.

## Updating the content

The HTML is hand-written and structured for direct editing — no templating, no markdown source.

| What you want to change | Where to edit |
|---|---|
| The "Current as of" date | Hero eyebrow (top of `<body>`) and footer colophon |
| The "What changed recently" cards | `<div class="context-grid">` near the top |
| A decision tree question or recommendation | The `STEPS` and `RESULTS` objects inside the `<script>` at the bottom |
| An exemption card | The `<div class="exemptions">` grid; each card is an `<article class="ex">` |
| A working note | The `<div class="notes-list">` inside the `#notes` section |
| Outbound link URLs | The `<section id="resources">` block, and the various `.ex-foot` links inside each card |

Preview by opening `index.html` in a browser. Commit and push to redeploy.

## Contributing

Updates from QWSG members and partner agencies are welcome. The preferred workflow:

1. **Open an issue** describing the statute change, broken link, or content gap
2. **Submit a pull request** for review by the Environmental Compliance Subcommittee
3. **Subcommittee approval** before merging — content changes touch a public-facing reference, so review is intentional

For non-trivial changes (new pathway, restructured tree, major reframing), please flag it at the next Subcommittee meeting before opening the PR.

## Credits & license

Maintained by the Environmental Compliance Subcommittee of the Nevada County Quarterly Wildfire Stakeholders Group.

Released to the public for re-use and adaptation by other stakeholder groups, RCDs, fire safe councils, and agencies. Please retain attribution to the Subcommittee and link back to this repo when redistributing or adapting.

## Contact

For questions about the content, statute interpretation, or how a pathway applies to a specific project, contact the Environmental Compliance Subcommittee through the Nevada County QWSG.

For website issues (broken links, typos, display problems), please open a GitHub issue.

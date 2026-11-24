# CEQA Decision Pathways — pathway-finder redesign (Oct 2026)

> Moved here from the Yuba Watershed Institute site repo in Nov 2026. References to `lib/resources.ts` and other `ywi-nextjs` paths describe that repo at the time of the redesign.

Design notes for the redesign of `index.html` § 01. The old
"master decision tree" asked five yes/no questions and landed on one of six general
mechanisms (not a project / federal-NEPA / "file an NOE" / tier from a PEIR / Forest
Practice / standard document). The "file an NOE" answer left the hard part — *which*
exemption — to the reader. The redesign replaces the tree with a **project profile**
and a **rule engine** that scores every pathway on the menu.

This is a prototype for Permitting & Environmental Compliance subcommittee review. Nothing
here is legal advice; the rules encode a working reading of the statutes cited below and
need a practitioner's eye before the page goes live.

## What changed on the page

| Area | Before | After |
|---|---|---|
| § 01 questions | 5 yes/no | up to 14 single- or multi-choice questions with skip logic |
| Result | one of 6 general mechanisms | a headline pathway **plus** every relevant pathway ranked *Likely fit / Possible — confirm / Unlikely*, with the specific conditions met, unmet, still to confirm, and the follow-through steps |
| Pathways scored | — | 16 CEQA pathways (federal-NEPA; SB 131 a–d; SERP; existing document; Class 33, 4, 1, 7/8; declared emergency; planning study; SB 1370; CalVTP; standard fallback) + 7 Forest Practice options (§1038 b/c/d/e/g, §1038.3, THP/NTMP/WFMP/PTEIR) |
| Shelf life | — | sunset reminders keyed to the start-date answer |
| Output | — | "Copy summary for memo to file" — plain text of answers, fits and conditions |
| Exemption menu | unchanged structure | cards get `id="ex-1"`…`ex-8` so fits deep-link; Forest Practice card corrected (see below) |

### Content corrections made while wiring the rules (flag for review)

- **§1038.3 Forest Resilience Exemption: 300 ac → 500 ac.** The current regulation text
  (14 CCR §1038.3, amended Jan 2025) caps the harvest area at **500 acres**; the 300-ac
  figure was the pre-AB 2276 Forest Fire Prevention Exemption. Also added the <30″ DBH
  limit, large-tree / canopy retention, and the Jan 1, 2031 sunset.
- Added **§1038(c)(6)** (150–300 ft from a habitable structure with stocking / QMD / RPF
  fire-behavior certification) and **§1038(g)** (wildfire-killed trees within 300 ft) to
  the Forest Practice card, because the engine references both.
- **§1038(e) Oak Woodland Exemption card** no longer says the 30″ conifer limit is "rulemaking in
  progress": the current 14 CCR §1038(e) text (operative Jan 15, 2025) carries the 30″ limit, the
  RPF-justification escape hatch, and the ≥80 % oak basal-area retention. The 300 ac per
  ownership per planning watershed per 5-year cap is confirmed against the same text.
- `lib/resources.ts` blurb updated to describe the profile instead of "five Yes/No
  questions". The "current as of October 2026" date is unchanged.

## The profile (questions)

| # | id | Asked when | Drives |
|---|---|---|---|
| 1 | `trigger` — who approves, funds or carries out the work | always | CEQA "project" test; lead-agency type (Classes 7/8 need a regulatory agency); CalVTP proponent |
| 2 | `land` — federal / private / public / mixed | always | §4799.05(d); Forest Practice Act (non-federal only); CalVTP treatable landscape |
| 3 | `nepa` — NEPA status | federal or mixed | §4799.05(d) |
| 4 | `activities` (multi) — thin, commercial, Rx fire, defensible space, roadside, fuel break, restoration, hazard trees, herbicide, new roads, plan only | always | gates nearly every pathway |
| 5 | `acres` — 7 bands at the statutory thresholds (5 / 50 / 300 / 500 / 1,000 / 3,000) | not plan-only | Class 33 (≤5), SB 131(a) (≤50), §1038(e) (≤300), §1038(d)/§1038.3 (≤500), SB 1370 (<1,000), Class 4 scale judgment |
| 6 | `structures` — distance bands 30 / 100 / 150 / 200 / 300 ft | not plan-only | Class 4 §15304(i), §1038(c), SB 131(c)(d), SB 1370 defensible space |
| 7 | `dbh` — largest trees cut: <12″ / 12–30″ / >30″ | vegetation removal selected | SB 131(b)(d) 12″ limit; §1038.3 and §1038(e) 30″ limit |
| 8 | `community` (multi) — evacuation route, ½ mi of 30+ DU subdivision, WUI, none | not plan-only | SB 131(a)(b); SB 1370 roadside |
| 9 | `hazard` — FHSZ rating × SRA/LRA/FRA | not plan-only | SB 131(c) (High/VH); §1038.3 (Mod/High/VH); CalVTP (SRA now, LRA after the update) |
| 10 | `resources` (multi) — listed species / critical habitat, riparian, chaparral/CSS, cultural, scenic highway, Cortese, steep slopes, not checked, none | not plan-only | §15300.2 exceptions; SB 131(a) habitat carve-out; SB 1370 take-permit bar; CalVTP writ; Class 33 conditions |
| 11 | `purpose` — can it be designed and described as habitat restoration? | thinning, Rx fire, fuel break, herbicide or restoration selected | SERP "exclusively" test (purpose and design, not activity type); Class 33 |
| 12 | `plans` (multi) — identified in an adopted plan, already covered by a CEQA document, maintenance of a prior treatment, declared emergency | always | SB 1370 plan requirement; existing-document path; Class 1; §15269 |
| 13 | `timing` — start before 2027 / 2027 / 2028 / 2029+ | always | sunsets: SB 1370 window (Jan 1 2027 – Jan 1 2029), §4799.05(d) (Jan 1 2028), SERP (Jan 1 2030), §1038.3 (Jan 1 2031) |
| 14 | `timber` (multi) — oak woodland, dead/dying, drought mortality, fire-killed, existing plan | commercial on non-federal land | which §1038 notice exemption |

Changing an earlier answer never discards later ones; questions that are no longer
applicable are simply ignored by the rules.

## How fit is scored

Each pathway has a `gate(profile)` (is it even relevant to this project?) and a
`test(profile)` returning findings of four kinds:

| kind | meaning | effect on fit |
|---|---|---|
| `ok` | a condition the profile satisfies | — |
| `chk` | a condition the profile cannot verify, or a judgment call | caps fit at **Possible** |
| `no` | a condition the profile fails | **Unlikely** |
| `doc` | follow-through once you pick it (file the NOE, get CDFW concurrence, RPF marking…) | none |

Fit = Unlikely if any `no`, else Possible if any `chk`, else **Likely**. The standard
document is a fixed "Fallback". Pathways whose gate fails are not shown at all (e.g.
Class 33 when no restoration is selected), which keeps the list short. When the profile
says CEQA does not apply (`trigger = none`), only the Forest Practice options are scored.

The headline is the first Likely pathway in this preference order, else the first
Possible: federal-NEPA → SB 131 (c), (d), (b), (a) → SERP → existing document → Class
33 → Class 4 → Class 1 → Class 7/8 → emergency → planning study → SB 1370 → CalVTP →
standard. Statutory exemptions lead because §15300.2 does not reach them; SB 1370 sits
behind the categoricals because it is a state application with a two-year window and no
posted process yet; CalVTP sits last among the streamlined options because a PSA is the
heaviest of them. If the project has a commercial component on non-federal land, the
headline also names the closest Forest Practice fit as the foundation.

### Rule matrix (what each pathway checks)

Statute text verified Oct 2026 against the sources listed at the end; the SB 1370 and
AB 1699 entries rely on the page's existing Oct 2026 content (bill text on leginfo).

| Pathway | Hard conditions (`no` if failed) | Unverifiable / judgment (`chk`) |
|---|---|---|
| **§4799.05(d)** federal-NEPA | federal land; NEPA complete; approval before Jan 1, 2028 | mixed ownership covers the federal part only; NEPA underway; agency role is funding/staffing or GNA; new roads must be in the NEPA project description |
| **SB 131 (a)** | ≤50 contiguous ac; within ½ mi of 30+ DU subdivision; not coastal sage scrub / sensitive habitat | listed-species site may be "sensitive habitat"; fuel reduction must be framed as re-establishing the fire return interval |
| **SB 131 (b)** | road is an identified evacuation route for 30+ DU; trees <12″ DBH | — |
| **SB 131 (c)** | within 200 ft of a legal structure; High or Very High FHSZ | defensible-space framing |
| **SB 131 (d)** | within 200 ft of structures; trees <12″ DBH | fuel-break framing |
| **SERP** §21080.56 | purpose is structure protection / defensible space / evacuation corridor; no non-habitat construction | fuels-driven project that could be redesigned around habitat (Sierra Foothill precedent); defensible-space or roadside components; commercial sale; determinations before Jan 1, 2030 |
| **Existing document** | — | §15162 no-new-impacts test |
| **Class 33** §15333 | ≤5 ac; purpose is structure protection / defensible space / evacuation corridor; no hazardous materials | fuels-driven project that could be redesigned around habitat; listed species impact not significant |
| **Class 4** §15304 | no new roads/major grading; >300 ac; mapped critical habitat / listed species (§15300.2(a)); scenic highway; Cortese list | Rx fire only; herbicide; 50–300 ac; cultural, riparian, steep slopes; records not checked |
| **Class 1** §15301 | must be maintenance of an existing treatment; no herbicide (§15301(h)); no new roads; scenic / Cortese | cultural; listed species as unusual circumstance |
| **Class 7/8** | regulatory agency lead; no road construction | "construction activities" exclusion |
| **§15269 emergency** | a current declaration covers the work | near-term action, not a long-term project (§15269(c)) |
| **§15262 planning** | — | — |
| **SB 1370** §§4240–4243 | start inside Jan 1, 2027 – Jan 1, 2029; not "none" for plan identification; no pesticides; ≥1,000 ac forest; Rx fire in chaparral/CSS; defensible space >200 ft | plan identification unknown (CWPP priority areas?); CESA/NPPA take permit; fuel break ≤300 ft wide; roadside ≤100 ft; federal land |
| **CalVTP PEIR** | federal responsibility area | non-public proponent; LRA until the SPEIR is certified; commercial timber; new roads; chaparral/CSS (SPR CHAP-CSS-1) |
| **§1038(c)** | >300 ft from structures | 150–300 ft under (c)(6) |
| **§1038(b)** | — | dead/dying/diseased <10 % of volume |
| **§1038(d)** | >500 ac | — |
| **§1038(e)** | >300 ac per ownership per watershed per 5 yr | oak woodland stand conditions; conifers >30″ only where necessary |
| **§1038.3** | FHSZ not Mod/High/VH; >500 ac; live trees >30″ | — |
| **§1038(g)** | — | — |
| **THP/NTMP/WFMP/PTEIR** | — | always "possible" unless an approved plan exists |

Side notes that print regardless of pathway: RPF engagement (commercial), burn permits and
the AB 1699 §21080.49.1 nuance (Rx fire), §1602 / 401 permits (riparian), CESA take
(listed), records searches (unsurveyed), PRC §21083.4 oak check, herbicide closing Class 1
and SB 1370, and the NOE / 35-day limitations reminder.

## Open questions for the subcommittee

1. **Class 4 and mapped critical habitat.** The engine treats a documented listed-species
   occurrence or mapped critical habitat as defeating Class 4 under §15300.2(a). Some
   practitioners would call that "possible with documentation" rather than "unlikely".
   Which reading should the page take?
2. **SERP gate widened (Nov 2026).** SERP is now scored for any thinning, prescribed fire,
   fuel-break, herbicide or restoration project, and the purpose question asks whether the
   project can be *designed and described* as habitat restoration. Basis: CDFW concurrence
   21080.56-2023-034-R2 (Aug 7, 2023) for the Sierra Foothill Forest Climate Resilience
   Project — WCB lead, Sierra Streams Institute, 625 ac of thinning, mastication, prescribed
   fire and herbicide across the Jones Bar Firewise community and BLM land — which treated
   wildfire-risk reduction as the §21080.56(b) incidental public benefit. It is not a lone
   precedent. Four later concurrence letters were read for this note and follow the same
   pattern — a named set of beneficiary species, a fire-regime or forest-structure restoration
   rationale under (a), public safety to neighbouring homes and infrastructure as the (b)
   incidental benefit, BMPs plus monitoring and a maintenance plan under (c), and "no
   construction" under (d):
   - **21080.56-2024-055-R3** Lodge Road Wildfire Resilience and Large Tree Restoration
     Demonstration Project (State Parks, Big Basin, 54 ac, Jul 2024): mechanical thinning of
     second-growth redwood to ~24 in. DBH with heavy equipment, mastication, tractor crushing,
     pile and broadcast burning; purpose is late-seral habitat for marbled murrelet; hazardous
     dead-tree removal along Lodge Road is the incidental benefit.
   - **21080.56-2024-065-R1** Skey-wok Kee' We Mech (It Needs Fire) Phase 1 (CAL FIRE lead,
     Cultural Fire Management Council, Yurok Reservation, 1,173 ac, Dec 2024): cultural and
     prescribed fire with manual thinning to restore historical fire regimes in
     conifer-encroached oak woodland and prairie; funded under CAL FIRE's Forest Health grant
     program — CAL FIRE itself used SERP as lead agency for a fuels-grant project.
   - **21080.56-2025-082-R6** Wildwood Canyon Wildfire Resilience Project (State Parks with
     CAL FIRE and IERCD, Yucaipa, 161 ac, Dec 2025): prescribed fire, mechanical thinning,
     mastication, chipping, goldspotted-oak-borer tree removal and possible herbicide, for
     Crotch's bumble bee, burrowing owl, spadefoot toad and two rare plants. Notably it
     includes "infrastructure protection via manual removal of vegetation" *inside* the
     restoration design, and was initiated by a local fire safe council.
   - **21080.56-2026-111-R6** Numu Tuna'e (The People's Burn) Beneficial Fire Project (CAL FIRE
     lead with the Mono Lake Kootzaduka'a Tribe and LADWP, 6 ac, Nov 2026): beneficial fire in
     willow riparian habitat with hand-tool control-line preparation.
   Scale ran from 6 to 1,173 acres; tools included heavy equipment and herbicide; two of the
   four lead agencies were CAL FIRE. A fuels-driven
   project that *could* be redesigned scores *Possible*; structure-protection, defensible-space
   and evacuation-route purposes score *Unlikely*. Is the "designed that way" bar described
   clearly enough for a grant writer to self-assess?
3. **SB 1370 plan identification.** Whether a CWPP *priority area* "identifies" a project is
   unresolved; the engine flags it as "confirm" rather than guessing.
4. **CalVTP in the LRA.** Treated as "possible" pending the Subsequent PEIR. Should it be
   "unlikely" until certification?
5. **Headline ordering.** Statutory → categorical → SB 1370 → CalVTP → standard. Reasonable
   default for Nevada County projects?
6. **Should Class 4 reach "Likely" at all?** Today a clean records check plus a small
   footprint produces *Likely*; the §15300.2(b)/(c) reminders print as follow-through.
   An alternative is to cap categoricals at *Possible* always.
7. **Class 4 scale cut-offs are judgment calls.** §15304 has no acreage cap. The engine marks
   50–300 ac as *confirm* and >300 ac as *unlikely* ("a minor-alteration finding is hard to
   defend at this scale"). Keep, loosen, or drop the 300-ac line?
8. **Pre-existing, out of scope:** the site CSP (`next.config.js` `style-src` / `font-src`)
   does not allow `fonts.googleapis.com` / `fonts.gstatic.com`, so the page's Newsreader /
   IBM Plex fonts are blocked and it renders in fallback fonts. Either self-host the fonts
   (as `public/maps` does) or add the origins.

## Maintaining the engine

- Questions live in `QUESTIONS[]`, pathways in `PATHWAYS[]`, inside the last `<script>` in
  `index.html`. Add a pathway by adding one object with `gate` and
  `test`; use `ok / chk / no / doc` and the tri-state helpers `acresLE`, `withinFt`,
  `dbhLE`, `zone`, `area`, `startYear`.
- A pathway's `card` is the `#ex-N` anchor of its exemption-menu card — keep them in sync.
- `node --check` the inline script after every edit (extract it or paste it into a file);
  a syntax error silently kills the whole § 01 pane.
- Sunset years are hard-coded in `timing` logic and the `shelfLife()` strings; when a
  sunset moves, change both plus the card.

## Verification done

- Inline script passes `node --check`; `npx tsc --noEmit` and `npm run lint` clean.
- Seven scripted scenarios in the browser (county defensible space near a subdivision;
  private commercial oak release with no public money; state-funded federal-land thinning
  with NEPA done; RCD meadow restoration ≤5 ac; 1,500-ac RCD thinning + Rx fire with
  herbicide and listed species; plan only; a half-answered profile). Headlines: SB 131(c)
  likely; not a CEQA project + §1038(e) likely; federal-NEPA likely; SERP likely (Class 33
  also likely); CalVTP likely with SB 1370 and Class 4 unlikely; planning study; and
  a "Preliminary · 3 / 11 answered" ranking.
- Desktop (1280 px) and phone-width layouts screenshotted; no console errors from the
  script (the only console errors are the pre-existing font CSP block and the local
  Vercel insights 404).

## Sources used for the rules

- PRC §21080.49 (SB 131, 2025) — text via FindLaw (current to Jan 1, 2026).
- PRC §21080.56 (SERP; SB 174 extension to Jan 1, 2030) — FindLaw; CDFW SERP Q&A page.
- CDFW SERP concurrence 21080.56-2023-034-R2, Sierra Foothill Forest Climate Resilience Project
  (nrm.dfg.ca.gov DocumentID 213746) and its CEQAnet record SCH 2023080762.
- CDFW SERP concurrences 21080.56-2024-055-R3 (DocumentID 225142), 21080.56-2024-065-R1
  (227957), 21080.56-2025-082-R6 (239873) and 21080.56-2026-111-R6 (247932), all read Nov 2026.
- PRC §4799.05(d) (SB 901 / AB 211; inoperative Jan 1, 2028) — FindLaw.
- 14 CCR §§15300.2, 15301, 15304, 15307, 15308, 15333 — Cornell LII.
- 14 CCR §1038 and §1038.3 (as amended Jan 2025) — Cornell LII.
- SB 1370 (Ch. 777, Stats. 2026) and AB 1699 (Ch. 761, Stats. 2026) — the page's existing
  Oct 2026 summaries; leginfo could not be fetched directly this session.
- CalVTP PEIR scope — Board of Forestry PSA documents (public-agency proponent; SRA
  treatable landscape; three treatment types; five activities).
- Ascent, "Streamlining Synopsis: Expedited Environmental Review Opportunities for
  Wildfire Safety Projects" — cross-check of thresholds.

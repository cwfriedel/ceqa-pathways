# Contributing to CEQA Decision Pathways

Thanks for helping keep this reference accurate. It is maintained for the
**Permitting & Environmental Compliance Subcommittee** of the Nevada County Quarterly
Wildfire Stakeholders Group (QWSG), and it is public-facing, so every content change is
reviewed by the subcommittee before it goes live.

## Ways to help

**Report an error or suggest a change (no coding needed).**
- With a GitHub account: [open an issue](https://github.com/cwfriedel/ceqa-pathways/issues/new/choose).
  Pick "Correction" for something wrong or out of date, or "Suggestion" for anything else.
- Without one: use the "Suggest a change" link in the page footer, or raise it with the
  subcommittee through the QWSG.

Please include a **primary source** (bill text on leginfo.legislature.ca.gov, the statute
or regulation, or the agency page) for any legal or status claim. Secondary summaries
are useful leads but not enough on their own.

**Propose an edit directly.** Fork the repo, edit `index.html`, and open a pull request.
`CLAUDE.md` has a table of where each kind of content lives and the checks to run.

## How review works

1. A pull request is opened (by a contributor or the maintainer).
2. Vercel builds a **preview link** for it automatically and posts it on the pull request.
   The preview shows a yellow "draft, pending subcommittee review" banner so it can't be
   mistaken for the live reference.
3. The maintainer emails that preview link to the subcommittee. Reviewers need only a web
   browser, not a GitHub account.
4. Once the subcommittee signs off (at a meeting or by email), the maintainer notes who
   approved and when on the pull request, then merges it.
5. Merging to `main` publishes the change to <https://ceqa-pathways.vercel.app> and, through
   it, to the Yuba Watershed Institute website.

Non-trivial changes (a new pathway, a restructured pathway finder, a major reframing)
should be raised at a subcommittee meeting before the pull request is opened.

## Accuracy rules

- Distinguish enacted law from pending bills, and label pending items "pending, not yet
  law". Prefer "no recorded action since <date>" over asserting that a bill is dead.
- Keep the "not legal advice" framing (footer disclaimer and the note in Resources).
- See `CLAUDE.md` for known-fixed errors that must not come back (for example, the
  federal-NEPA exemption is PRC §4799.05(d) from SB 901 / AB 211; AB 404 failed).

## License

By contributing, you agree that your contributions are licensed under the project's
licenses: CC BY 4.0 for content and MIT for code. See `LICENSE`.

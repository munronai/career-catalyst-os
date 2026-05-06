# job-search skill

*A Claude Code skill for individual job-seekers researching their own job applications. Reads from a profile, runs site-scoped searches across major ATS platforms, deduplicates against previously-seen listings, and produces match/gap analyses against the profile.*

**Status:** Personal-productivity tool, published as a reference implementation and learning-in-public artefact. Not a product, not a service, not maintained against an SLA.

**Author:** Neil Munro. The skill is published under the author's personal GitHub account as part of an ongoing exploration of AI Product Management methodology, regulatory frameworks (EU AI Act, ISO 42001), and the practical use of Claude Code for PM workflows.

---

## What this is — and what it is not

**This skill is built for one thing:** helping an individual job-seeker analyse how their own profile fits roles they are considering applying for.

**This skill is not built for, and must not be used for:**

> **Recruitment, candidate screening, hiring decisions, or any AI-assisted evaluation of natural persons by a third party.**
>
> If you are a recruiter, hiring professional, talent platform, or considering using this tool to evaluate candidates: please don't. Such use places the deployment into the EU AI Act's high-risk regime under Annex III(4)(a) and engages substantial GDPR/UK GDPR obligations. This implementation has none of the controls those obligations require. Repurposing the skill for recruitment would make the deployer a *provider* under Article 25(1)(c) of the EU AI Act, inheriting full provider obligations on top of their deployer obligations.
>
> Recruitment-grade AI tools exist that have done the regulatory work properly. Use one of those.

This is not boilerplate. It is the binding statement of intended use that scopes both the technical design of the skill and the legal posture of the project. See `USAGE.md` for the full intended-use statement and out-of-scope list.

## What the skill does

A typical session: the user invokes the skill from Claude Code, the skill reads `profile.md` and `search-config.md`, runs site-scoped searches across configured ATS platforms (Greenhouse, Lever, Ashby, etc.), deduplicates against `~/job-search/seen_jobs.json`, applies a freshness window, and returns a ranked list of candidate roles with match/gap analyses against the profile.

A variant invocation: the user provides a single role description directly, and the skill produces a match/gap analysis without running the search.

That's it. There is no telemetry, no persistent server, no cloud component beyond the Anthropic API call that the skill's prompts trigger. Everything else is local files in your skill directory.

## How to use it

See `SKILL.md` for the operating manual, `PRIVACY.md` for the data-flow documentation, and `USAGE.md` for intended-use statements and recommendations on profile contents.

A short summary:
1. Edit `profile.md` to describe yourself in terms that will match against role descriptions. See `USAGE.md` for what to include and what not to include — data minimisation matters here.
2. Edit `search-config.md` to set your search terms, target sites, and freshness window.
3. Invoke the skill from Claude Code.
4. Review the output. Decide where to apply.

## A note on data and privacy

When the skill runs, your profile is sent to the Anthropic API as part of the prompts the skill issues. Anthropic processes this data under its own published policies — read them at https://www.anthropic.com/legal . The skill itself does not retain, transmit, or share data beyond the API calls it makes and the local files it reads and writes.

If your profile contains personal data — and most useful profiles do, even without obvious identifiers — you are sending that personal data to Anthropic when you run the skill. Read `PRIVACY.md` before deciding what your profile contains.

## A note on the regulations referenced above

The skill exists as part of the author's wider work on AI regulatory frameworks for product managers. Several documents in this repo (`PRIVACY.md`, `USAGE.md`, the intended-use statement above) are written more thoroughly than a personal-productivity tool would normally warrant. This is deliberate: they are also worked examples of how the author thinks about the EU AI Act's provider obligations, the GDPR's data-minimisation principle, and ISO/IEC 42001's intended-use documentation requirements. If they are useful to you for that secondary purpose, take what helps.

## License

See `LICENSE`. The license includes a use restriction prohibiting use of this software for AI-assisted recruitment, candidate screening, or evaluation of natural persons by a third party. This restriction may not be fully enforceable in all jurisdictions, but it represents the author's clear position on intended use, reinforces the regulatory analysis above, and provides a contractual basis for requesting cessation of misuse.

## Reporting misuse

If you become aware of this skill being used for purposes outside its documented intended use — particularly recruitment, candidate screening, or evaluation of natural persons by a third party — please open an issue. The author engages with reasonable removal requests and will publicly disengage from such uses.

## Disclaimers

This is a personal-productivity tool, offered as-is, without warranty of any kind, express or implied. The author is not a lawyer; documentation in this repo describes the author's good-faith understanding of the relevant regulatory frameworks and is not legal advice. If you need legal advice about whether or how to use this skill in a specific context, consult a qualified lawyer.

The skill's outputs are AI-generated and are not authoritative. Match/gap analyses are heuristic; they reflect the LLM's reasoning over a profile and a job description and may be wrong. Decisions about your career remain yours.

## Versions

The intended-use statement applies to all versions and is binding from publication of the relevant version. Forks and modifications inherit the original intended-use statement unless the fork explicitly and substantially changes it (in which case, see Article 25(1)(c) — the modifier becomes the provider for that derivative work).

---

*Published as part of ongoing learning-in-public work on AI product management in regulated environments. Comments, corrections, and disagreements welcome via issues.*

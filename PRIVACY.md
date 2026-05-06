# Privacy notice — job-search skill

*This document tells you what data the skill handles, where that data goes, and what you control. It is written for users who install and run the skill on their own machine. The skill's author is not a privacy lawyer; this notice is a good-faith account of the data flows involved, not legal advice.*

**Last updated:** 30 April 2026

---

## What the skill is

The job-search skill is a Claude Code skill that searches for product management roles matching a candidate profile, deduplicates against jobs already seen, and produces match/gap analyses against the profile. It is published as a personal-productivity tool and a reference implementation of structured Claude Code skills. It is not a product, not a service, and not a SaaS offering.

## What data the skill handles

The skill processes three kinds of data, all stored locally on the machine you run it on:

**The profile (`profile.md`)** — a candidate profile that you author. The default contents are skills, experience, role preferences, and target sectors. You decide what goes in.

**The search configuration (`search-config.md`)** — keyword sets, target sites, freshness settings. No personal data unless you put it there.

**The seen-jobs cache (`seen_jobs.json`)** — a list of job listings the skill has already returned, used for deduplication. Contains job URLs, titles, employer names, and dates. This is information *about jobs*, not directly about you, but the pattern of jobs you've seen reveals information about your career interests and search behaviour.

## Where the data goes

When the skill runs, the following flows happen:

**The profile and the job description are sent to the Anthropic API** as part of the prompts the skill issues to Claude. This is necessary for the match/gap analysis. Anthropic processes this data under its API terms and data-handling commitments, which you should read at https://www.anthropic.com/legal . By default, Anthropic does not train models on API inputs; data is retained for a limited period for safety and operational purposes. If your profile contains personal data, that personal data is in the prompt sent to Anthropic.

**Web searches are issued to public job boards and ATS platforms** (Greenhouse, Lever, Ashby, etc.) using non-identifying search terms drawn from your search configuration. The job boards see only the search terms; they do not receive your profile.

**No data is sent anywhere else.** The skill has no telemetry, no analytics, no calls home to the author. There is no centralised store of profiles, no shared cache, no backup. Everything except the Anthropic API call and the public web searches stays on your machine.

## What is retained

**On your machine:** the profile, the configuration, the seen-jobs cache, and any search outputs you've saved. These persist until you delete them. They are stored as plain-text files in your skill directory.

**At Anthropic:** API request and response data is retained according to Anthropic's published policy (currently 30 days for most API tiers; check Anthropic's current documentation for the latest). You have no direct delete-from-Anthropic mechanism through this skill; if you need to exercise data subject rights against Anthropic, contact them directly.

**At the job boards you search:** the search terms you issued, retained according to each board's own policies. Standard web search hygiene applies; using a VPN, Tor, or a privacy-respecting search wrapper is your call.

## What you control

You can edit the profile at any time. You can delete the profile, the configuration, the cache, and any saved outputs at any time. You can run the skill without a profile (functionality will be reduced).

You can edit the prompts the skill uses (they are visible in the `SKILL.md`) to remove or transform fields before they are sent to the API.

You can run the skill against a deliberately stripped-down profile. See `USAGE.md` for guidance on data minimisation.

## What this notice does not cover

This notice covers data flows the skill itself causes. It does not cover:

- What you choose to do with the skill's outputs (sending them to recruiters, posting them publicly, etc.).
- What Anthropic, the job boards, or any other third party does with their copies of the data they received. Their terms apply.
- Anything you do outside the skill — for instance, what your operating system, browser, or shell is logging.

## Your role under data protection law

If you are running the skill on yourself, for yourself, with your own profile, you are simultaneously the data subject and the data controller. You are processing your own data under your own consent. Most data protection law does not actively apply in this scenario (the GDPR's "personal or household activity" exemption is the relevant concept under EU/UK law).

If you adapt the skill to process *other people's* data — for example, to research candidates for a role you are hiring for — you become a data controller in the conventional sense, and you should not assume this notice covers what you need. You will likely have your own GDPR/UK GDPR obligations including a lawful basis, transparency to the data subjects, and possibly a Data Protection Impact Assessment. **Using the skill for recruitment or candidate selection is also explicitly out of scope under the skill's intended use (see `USAGE.md`) and would likely bring the deployment into the EU AI Act's high-risk regime under Annex III(4)(a).** Don't.

## Author and contact

The skill is authored by Neil Munro, publishing under his personal GitHub account. The skill is offered as-is, without warranty. For questions about this notice, open an issue on the repository.

---

*Translation note. If you read the above and felt it was over-thorough for what is essentially "a script you run on your own laptop," that's accurate. The skill barely needs a privacy notice for solo personal use. The notice exists because publishing the skill on a public repo means other people may install and run it, and they deserve to know what they're agreeing to.*

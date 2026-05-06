# Usage guidance — job-search skill

*This document covers what the skill is for, what it isn't for, and how to use it well. Read alongside `PRIVACY.md` and `SKILL.md`.*

---

## Read this first

**This skill is not built for recruitment, candidate screening, hiring decisions, or any AI-assisted evaluation of natural persons by a third party.** Such use is explicitly out of scope, places the deployment into the EU AI Act's high-risk regime under Annex III(4)(a), engages substantial GDPR/UK GDPR obligations toward the affected candidates, and exposes the deployer to regulatory and legal risk that this implementation does not address.

If you are a recruiter, hiring professional, or talent platform considering using this tool: please don't. The match/gap functionality looks superficially similar to recruitment tooling but is built around a fundamentally different use case — a job-seeker analysing how their own profile fits a role they are considering. Recruitment-grade AI tools exist that have done the regulatory work properly. Use one of those.

---

## Intended use

This skill is intended for **individual job-seekers researching their own job applications**. Specifically:

- A person looking for their next role.
- Operating on their own behalf.
- Using a profile that describes themselves.
- Processing job listings that are publicly available.

If your use case fits this shape, you are using the skill as intended.

## Out of scope

The skill is **not** intended for, and must not be used for:

- **Recruitment or candidate screening on behalf of an employer.** This is a different use case and a fundamentally different regulatory regime. Recruitment AI in the EU is a high-risk AI system under Annex III(4)(a) of the EU AI Act, which engages provider and deployer obligations including risk management, data governance, technical documentation, transparency, human oversight, conformity assessment, and registration. This skill has none of the controls those obligations require. Adapting it without those controls is not a small modification; it is a different product. Such repurposing would also make the deployer a *provider* under Article 25(1)(c) — they would inherit provider obligations on top of their deployer obligations, because the substantial repurposing of an AI system into a high-risk use case transfers provider responsibility to the entity that did the repurposing.
- **Researching specific individuals other than yourself.** Don't put another person's profile into this skill, even if you believe you have legitimate interest. The skill is built around the assumption that the profile describes the user.
- **Production-grade automated job applications.** The skill produces match/gap analyses to help a human decide where to apply. It is not built for spray-and-pray automation.
- **Profiling at scale.** If you find yourself running this skill against many profiles, stop and reconsider what you are actually building.

## Recommended profile contents

The skill works best when the profile contains:

- **Skills and experience areas.** "API platform PM, fintech infrastructure, regulated environments." Specific enough to match against role descriptions.
- **Role preferences.** Seniority, IC vs management track, sectors, stage of company.
- **Geographic/remote preferences.** "UK remote or London hybrid."
- **Excluded categories.** "Not interested in traditional retail banks; not interested in junior titles."
- **Target compensation range** (optional, ranges work better than exact numbers).
- **Notable career achievements** stated as outcomes ("built API function from scratch at a UK challenger bank") rather than as resume bullets with names attached.

## Discouraged profile contents

The skill does not need, and you should not include:

- **Your full legal name.**
- **Email address, phone number, postal address, or any other direct contact identifier.**
- **Your current employer if your job search is confidential.**
- **Specific salary expectations as exact figures.** Use ranges if you must.
- **Specific dates of birth, ID numbers, or any other formal identifier.**
- **Genuinely sensitive personal data:** health information, religious affiliation, sexual orientation, political views, trade union membership. Profiles do not need this; the EU AI Act and GDPR treat this category with extra weight if you do include it.
- **Information about other people.** Manager names, colleague names, anything about people you have worked with. The profile is about you, not your network.

## Why this matters

Two things motivate the recommendations.

**Data minimisation.** Every piece of personal data in your profile is sent to the Anthropic API as part of the prompts the skill issues. Less data sent is less data exposed, retained, or potentially mishandled — by Anthropic, by anyone who could intercept the connection, by future you when you forget what's in the profile and accidentally share it.

**Re-identifiability.** Even without obvious identifiers (name, email), a sufficiently detailed profile is identifying. Career history with named former employers, distinctive project descriptions, skill combinations, geographic specifics — combined with any public signal (LinkedIn, GitHub, conference talks), a motivated identifier can re-identify a profile in minutes. This is a property of the data, not a flaw of the skill. The mitigation is to include enough detail to drive useful match analysis and no more.

## How to use the skill well

A few practical recommendations:

**Strip your profile before publishing the repo.** If you fork this skill and put your own profile into it, do not push that fork to a public repo with the profile committed. Use `.gitignore` for `profile.md` if you keep your fork connected to a public origin. The skill's repo includes a sample profile that is deliberately neutral; don't replace it with your own real profile if your fork will ever be public.

**Treat the seen-jobs cache as personal data.** It's a record of what you've been looking at. If you share your machine, store the cache in a location only you can access.

**Use the dry-run mode if you want to see what would be sent to the API before sending it.** (If your installation doesn't have a dry-run mode, that's a feature request worth raising in an issue — it's exactly the kind of transparency the IAGF framework recommends.)

**Re-read your profile periodically.** Profiles drift. Information that was fine to include three months ago may not be now. The skill encourages but does not enforce data minimisation; the discipline is yours.

## A note on the AI literacy duty

If you are using this skill in the context of your employer's work — for instance, to research roles for a role you are filling, or as part of a team's tooling — your employer has an EU AI Act Article 4 obligation (in force since 2 February 2025) to ensure staff using AI systems have appropriate AI literacy. This isn't burdensome and isn't course-shaped, but it is a real obligation. If you're using the skill personally for your own job search, this doesn't apply.

## What to do if something goes wrong

If you notice the skill behaving in a way that exposes personal data unexpectedly, processes data in a way the privacy notice doesn't describe, or otherwise surprises you in a privacy-relevant way, please open an issue on the repository. This is a personal-project skill, not a service with an SLA, but issues will be read and material privacy issues prioritised.

---

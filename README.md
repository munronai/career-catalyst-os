# career-catalyst-os

Agentic job search, interview, and application OS with governance using [`ai-rulez`](https://goldziher.github.io/ai-rulez/)

The *career-catalyst-os* builds upon the agent skills used in [career-catalyst-daily](https://github.com/munronai/career-catalyst-daily) aiming for an autonomous process with guardrails provided by the *ai-rulez* framework.
The guardrails help to control consistency across agents and to contain behaviour (e.g. recursive and expensive web searches from Claude Code when using *career-catalyst-daily*).

This repo contains the *.ai-rulez* configuration to generate the OS for both Claude Code and Gemini CLI, along with the skills listed below:

## :mag: job-search skill

Run a daily job search for Product Manager roles, with configurable search criteria, deduplication against previously seen listings, detailed match/gap analysis against a candidate profile, and keyword mapping per role. Trigger this skill whenever the user asks to find jobs, run a job search, check for new PM roles, look for work, show or edit their search criteria, or do anything related to their job hunt.

## 📈 role-prep skill

Prepares the candidate for applying and interviewing for a specific job role. Use this skill whenever the user selects a role from a job search, asks to prepare for an interview, wants to research a company before applying, needs help understanding what a role will involve, or asks for interview questions. Also triggers when the user says things like "help me prepare for this role", "what should I know about this company", "what questions will they ask me", "let's work on this one", or "I want to apply for [role]". Always use this skill when the user is moving from job search to application or interview preparation.

## :construction: proposal-builder skill

Rapidly prototypes and documents high-value proposals to support job applications.
  Turns role-prep ideas into "Proof of Value" packages including research, PRDs,
  strategy, architecture, and agent reviews.

**When the job search preparation completes, the OS offers suggestions for the candidate to tailor their CV/profile to each of the roles found and prepared.**

### Current Status

The *ai-rulez* generation has been tested for Gemini CLI and a successful execution recorded.

:exclamation: NOTE:

- *ai-rulez* does not fully copy the skills into the .claude or .gemini directories therefore some manual configuration is required
- For Gemini CLI, the skills are best copied into the .gemini/.agents/skills directory to override the global Gemini configuration (this is only necessary if the skills in *.ai-rulez* differ from the global skills)

## How to Use

### Setup

```bash
git clone https://github.com/munronai/career-catalyst-os.git
```

- Install `ai-rulez` as per the [instructions](https://goldziher.github.io/ai-rulez/installation/)
- Generate the agent configuration(s) using the command `ai-rulez generate`
- Copy the contents of the `.ai-rulez/skills` directory into the appropriate CLI skills config directory (e.g. into `.claude/skills` for Claude Code). Alternatively, add the skills directory contents into `.gemini/.agents/skills` for Gemini CLI
- Create a profile.md in the `job-search/references` directory that reflects your profile (Tip: use Claude to help you create this profile from the `job-search/references/profile-template.md`)
- Update the `job-search/references/default-search-config.md`. By default, this matches my requirements at the time this OS was created (Tip: use Claude/Gemini to guide you through the creation of a personalised set of search criteria)

### Run

Simply ask for the job search (e.g. "perform the daily job search please.")

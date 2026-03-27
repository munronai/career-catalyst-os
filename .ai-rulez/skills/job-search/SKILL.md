---
description: Run a daily job search for Product Manager roles, with configurable search criteria, deduplication against previously seen listings, detailed match/gap analysis against a candidate profile, and keyword mapping per role. Trigger this skill whenever the user asks to find jobs, run a job search, check for new PM roles, look for work, show or edit their search criteria, or do anything related to their job hunt.
name: job-search
---

# Job Search Skill

Finds new job postings that haven't appeared in a previous search, analyses each against the
candidate's profile, and surfaces keywords worth using. All search criteria are configurable
by the user and persist between sessions.

---

## User Commands

The skill responds to three types of request. Detect intent from the user's message:

### "Run a search" / "Find me jobs" / "Check for new roles"
→ Execute the full search pipeline: Steps 1–10 below. Results are presented in chat
  **and** saved as a dated markdown report to `~/job-search/reports/`.
  **Strict Compliance:** All steps must be followed exactly.

### "Show me my search criteria" / "What are my current search settings?"
→ Load `~/job-search/search-config.md` if it exists, otherwise load
  `references/default-search-config.md` from this skill. Display the full contents clearly to
  the user, labelled with its source:
  - If from user's file: _"Here are your current search criteria (from `~/job-search/search-config.md`):"_
  - If from defaults: _"You don't have a custom config yet. Here are the built-in defaults:"_

  After displaying, ask: _"Would you like to change anything?"_

### "Update my search criteria" / "Change the search settings" / "Edit [specific thing]"
→ If the user specifies what to change, make the edit. If they don't, show the current
  criteria first (as above) and ask what they'd like to modify.

  Changes are always saved back to `~/job-search/search-config.md`. If that file doesn't
  exist yet, create it by copying `references/default-search-config.md` and applying the
  changes. Confirm what was saved: _"Updated. Your new search criteria have been saved to
  `~/job-search/search-config.md`."_

---

## Step 1 — Load search config

Read `~/job-search/search-config.md`.

- **File exists** → use it as the active config for this session.
- **File missing** → read `references/default-search-config.md` from this skill folder and
  use it as the active config. Do not mention the fallback to the user during a search run —
  just use it silently.

The config defines: target titles, location, freshness window, strong signals, weak signals,
and search query templates. All subsequent steps use the active config, not hardcoded values.

---

## Step 2 — Load seen jobs

Read `~/job-search/seen_jobs.json`. If it doesn't exist, create it as `[]`.

URLs already in this file must be **silently skipped** — don't mention them, don't count them.

---

## Step 3 — Load candidate profile

Read `~/job-search/references/profile.md`.

- **File exists** → load fully into context for Steps 6 and 7.
- **File missing** → warn the user once at the top of your response:
  > ⚠️ No profile found at `~/job-search/references/profile.md`. Match/gap analysis and keyword mapping
  > will be skipped. See `references/profile-template.md` in this skill to get started.

  Then continue without analysis sections.

The profile must contain three sections: **Work History**, **Key Skills & Technologies**
(split into Strong / Familiar), and **What I'm Looking For**. See `references/profile-template.md`
for the expected structure.

---

## Step 4 — Search for jobs

Run 5–8 high-signal web searches using the query templates from the active config. Vary the angle each Vary the angle each
time — different queries should surface different companies and sources. Always include temporal
language ("last week", "past 6 days", "this week", current year) to bias toward fresh results.


**Efficiency Rule:** Review titles and snippets from search results first. Discard obvious mismatches, aggregators, or expired roles before proceeding to fetching. Deduplicate URLs across all search results.

---


## Step 5 — Batch fetch and validate results

For the filtered list of promising URLs: 

1. **Batch Fetch** — Use a single tool call to fetch the content for all URLs at once. This minimises user permission prompts and context turns.                                                                                           
2. **Validate** — For each fetched page:                                                                                            
- Confirm it's a real listing (not a 404 or category page)                                                                                            
- Check the posting date — skip anything older than the freshness window in the config                                                                         │
- Check the URL against `seen_jobs.json` — skip if already present                                                                                          - Score the fit against the strong/weak signals in the config

Hold in memory for each passing role:
- Title, company, location, posting date
- **Full job description text** — required for Steps 6 and 7
- **Salary / Compensation** — extract explicitly; if not listed, note "Not listed"
- Fit signals spotted
- Direct application URL

---

## Step 6 — Match/gap analysis

_(Skip this step if `profile.md` was not found in Step 3.)_

Compare the full JD against `profile.md`. Be specific and honest - a useful gap analysis is more valuable than a flaterring one. Provide a detailed breakdown of how the candidate's history satisfies the specific requirements of the JD.

### ✅ Matches & ⚠️ Gaps Table
Create a table comparing the candidate's profile to the JD requirements. Focus on the JD's specific detail.

| JD Requirement (Detail) | Status | Evidence / Bridge Strategy |
|---|---|---|
| [Specific quote or detail from JD] | ✅ Match | [Specific evidence from profile history/skills] |
| [Specific quote or detail from JD] | 〰️ Judgment | [Professional judgment: how adjacent experience satisfies the intent] |
| [Specific quote or detail from JD] | ⚠️ Gap | [Nature of the gap and whether it's bridgeable] |

**Judgment Rule:** Where a requirement doesn't explicitly match, make a professional judgment. If the user has adjacent experience, explain how this fulfill the *intent* of the JD (e.g., "Deep experience in ISO 20022 standards bridges the gap for the required Data Interoperability expertise").

---

## Step 7 — Keyword mapping (ATS Optimization)

_(Skip this step if `profile.md` was not found in Step 3.)_

Extract high-signal keywords and phrases from the JD that are likely to be used by AI screening agents (ATS) or recruiters. For each keyword, identify where in the user's profile there is a potential match or where a truthful reframing could increase interview chances.

For each keyword, classify it against the profile:

**✅ Usable — confirmed by profile**
The profile contains experience or skills that genuinely support this keyword. State exactly
where it's grounded (e.g. "Work History: [Company] — API governance work").

**〰️ Usable with caveat — partially supported**
The profile contains adjacent or related experience but not a direct match. Suggest honest
framing (e.g. "familiar with X through Y, not a primary owner").

**❌ Not usable — not supported by profile**
The profile provides no basis for this keyword. Flag it as a gap to develop or acknowledge
in interview — don't suggest the candidate use it.

Only include keywords where the classification adds value. Skip generic filler like
"strong communicator" — focus on terms with real signal for this role.

### 🔑 Keyword Mapping Table
| Keyword (ATS Signal) | Profile Potential | Actionable Reframing Suggestion |
|---|---|---|
| [e.g. "gRPC"] | ✅ Potential Match | [e.g. "Highlight gRPC usage in the KAO integration project"] |
| [e.g. "API Standards"] | ✅ Usable | [e.g. "Explicitly use 'API Standards' instead of 'Design Guides' in SWIFT section"] |
| [e.g. "PLG"] | ❌ Gap | [e.g. "Growth area; do not include in profile"] |

**Reframing Rule:** Suggestions must remain **truthful and authentic**. Focus on highlighting existing experience using the JD's specific terminology to pass automated filters. Skip generic filler—focus on terms with real signal.

---

## Step 8 — Present results

Format new jobs as a numbered list, ordered strongest to weakest fit. For each:

```
## [N]. [Job Title] — [Company]
📍 [Location]  🗓 Posted [date or "X days ago"]
💰 Salary: [Amount or "Not listed"]
🏷 [fit signal 1] · [fit signal 2] · [fit signal 3]
🔗 [URL]

### 📊 Match/Gap Analysis
| JD Requirement (Detail) | Status | Evidence / Bridge Strategy |
|---|---|---|
| ... | ... | ... |

### 🔑 Keyword Mapping
| Keyword (ATS Signal) | Profile Potential | Actionable Reframing Suggestion |
|---|---|---|
| ... | ... | ... |
```

Weak-signal roles (e.g. flagged employer type) go at the bottom:
> ⚠️ _[Flag reason] — included for completeness_

End with:
> _Found [N] new roles today. [M] previously seen listings were skipped._

---

## Step 9 — Save report to file

After presenting results in chat, write the **same output** to a dated markdown file.

**File path:** `~/job-search/reports/YYYY-MM-DD.md` using today's actual date.

If a report for today already exists (e.g. from an earlier run the same day), append the new
results to it under a new `---` divider with a run timestamp, rather than overwriting.

**Report format:**

```markdown
# Job Search Report — [Day, DD Month YYYY]

_Search config: [brief description e.g. "Senior/Lead/Staff/Principal PM · API/platform · UK remote"]_
_Profile: [candidate name or "loaded" if profile.md exists, "not loaded" if missing]_

---

[Full results content — identical to chat output, including all role entries,
match/gap analysis, keyword tables, flags, and the summary line]
```

Create `~/job-search/reports/` if it doesn't exist.

After saving, confirm to the user:
> _Report saved to `~/job-search/reports/YYYY-MM-DD.md`_

---

## Step 10 — Update seen_jobs.json

Append all newly presented job URLs to `~/job-search/seen_jobs.json`. Never remove old entries.

Create `~/job-search/` if it doesn't exist.

---

## Notes & Tips

- **Reports**: Each search run saves a dated `.md` file to `~/job-search/reports/`. Multiple
  runs on the same day append under a divider. Past reports can be reviewed to track which
  roles have been applied to, followed up on, or closed.
- **ATS domains**: Greenhouse (`greenhouse.io`), Lever (`lever.co`), Ashby (`ashbyhq.com`),
  Workday, SmartRecruiters, Rippling
- **False positives**: Skip aggregator pages and listicles unless they link to a real listing
- **Date ambiguity**: Include but flag with ⚠️ if posting date can't be confirmed
- **Salary**: Include in the summary if visible; flag if clearly outside profile expectations
- **Hacker News "Who is Hiring"**: Monthly thread often surfaces niche technical PM roles
  not on job boards — worth including in the search rotation
- **Reset seen jobs**: If the user asks to clear history, empty `seen_jobs.json`
- **Day-specific filters**: If the user adds one-off filters for a run, apply them as an
  extra pass after Step 5, without saving them to the config

---
name: role-prep
description: >
  Prepares the candidate for applying and interviewing for a specific job role. Use this skill
  whenever the user selects a role from a job search, asks to prepare for an interview, wants
  to research a company before applying, needs help understanding what a role will involve, or
  asks for interview questions. Also triggers when the user says things like "help me prepare
  for this role", "what should I know about this company", "what questions will they ask me",
  "let's work on this one", or "I want to apply for [role]". Always use this skill when the
  user is moving from job search to application or interview preparation.
---
 
# Role Preparation Skill
 
Turns a job listing into a structured preparation document: company intelligence, role
reality check, 6-month proposal ideas, and interview questions tailored to each interviewer
type. Output is saved as a markdown file and presented in chat.
 
This skill chains from the **job-search** skill (roles surface there first) and feeds into
the **proposal-builder** skill (coming later), where the candidate develops one of the
proposed strategies into a full PRD, user stories, and prototype.
 
---
 
## Input
 
The user can refer to a role in any of these ways:
 
- **By number** from the most recent search report — e.g. "Role 3" or "the Ebury one"
- **By URL** — paste a job listing link
- **By description** — company name, role title, or both
- **Deep Dive / Comprehensive Mode:** Explicitly requested via keywords like "deep dive", "comprehensive", "full research", or "research more".
 
If the reference is ambiguous (e.g. "the API one" could match multiple roles), ask the user
to clarify before proceeding.
 
If the user provides a URL, fetch it directly. If they reference a report role, load the
most recent report from `./.ai-rulez/skills/job-search/references/reports/` to find the URL, then fetch the full JD.
 
---
 
## Step 1 — Resolve and load the job listing
 
1. Identify which role the user means (see Input above)
2. Fetch the full job description from the direct application URL
3. Extract and hold in memory:
   - Role title and seniority
   - Company name
   - Location / remote policy
   - Key responsibilities (verbatim, for later analysis)
   - Requirements and nice-to-haves
   - Any salary or comp information
   - Team context (team size, reporting line, tech stack) if mentioned
 
If the JD can't be fetched (gated behind login, 404, etc.), ask the user to paste the
job description text directly.
 
---
 
## Step 2 — Research the company
 
Execute the research phase, adapting depth based on whether a "Deep Dive" was requested and the scale of the organization.
 
### Standard Research (5–7 searches)
Cover fundamentals with a focus on **Strategic Context**:
- **Business Model & Scale:** Product, customers, pricing, and revenue/growth signals (ARR, funding, or public filings).
- **Competitive Dynamics:** Who are the incumbents vs. challengers? What is the company's "moat"?
- **Product Velocity:** Recent launches, feature ship-rate, and current "Product Voice" from leadership.
- **AI Signal Detection:** Identify if the role/company has an active AI roadmap, agentic surface, or if AI is mentioned in the JD.
- **Culture:** Glassdoor/Blind signals and stated values.

### Deep Dive Research (10–14 searches)
If requested, expand search to cover:
- **Leadership & Vision (Scale-Aware):**
  - *Startups/Scale-ups (<500 employees):* Focus on Founders/C-Suite personal "Product Voice" (podcasts, talks, blogs).
  - *Enterprise (>500 employees):* Focus on Business Unit (BU) Vision and Strategic Pillars. Use IR decks, annual reports, and keynote talks from BU leaders rather than the group CEO.
- **Video & Media Intelligence:** Summarize "Product Voice" from YouTube interviews, conference talks, or webinars.
- **User & Market Sentiment:** Aggregate reviews from appropriate sources (G2, Trustpilot, App Stores). Identify specific **"Delighters"** (what users love) vs. **"Friction Points"** (recurring pain points/churn drivers).
- **Technical & Operational Hurdles:** Identify "Real Problems" being solved *now* (e.g., scaling to a new market, technical migration, or regulatory deadlines).
 
Summarise findings concisely. Flag green flags or potential concerns.
 
---
 
## Step 3 — Role reality check
 
Based on the JD and company research, write a clear-eyed summary of what this role will
**actually** involve day-to-day.
 
Structure this as:
 
### What you'll spend most of your time on
The realistic 70–80% of the role. Call out if "strategy" in the JD likely means "writing tickets and unblocking engineers" in reality.
 
### What success looks like in 3–6 months
Implicit metrics or milestones likely expected by the hiring manager.
 
### Where this role could go
Career trajectory, autonomy, and influence signals.
 
### Flags worth noting
Concerns given the candidate's profile and preferences (e.g. office policy, domain shift, churn signs).
 
---
 
## Step 4 — Three 6-month proposals
 
Generate **3 distinct proposals** grounded in the research. 
 
**Deep Dive Rule:** If in Deep Dive mode, proposals MUST explicitly address either a **Strategic Priority** (from leadership research) or a **Friction Point** (from user sentiment research) identified in Step 2.
 
For each proposal, use this structure:
 
```
### Proposal [N]: [Short title]
 
**The opportunity:** [What problem or gap does this address, and why now?]
 
**What you'd do:** [3–5 concrete activities or workstreams]
 
**Why you're well-placed to lead this:** [Link to candidate's profile — specific experience or skills]
 
**How you'd measure success:** [1–3 specific, observable outcomes at 6 months]
 
**Dependencies and risks:** [What could slow this down or require buy-in?]
```
 
---

## Step 5 — Product Sense Deep Dive (The 5 Thinking Shifts)

Generate a "Hero Answer" to a high-probability product sense question (e.g., "How would you grow X?" or "Design Y for the company"). If an AI Signal was detected in Step 2, use the **AI Overlay** version. Otherwise, apply the shifts to the **Core Product**.

### Shift 1: Strategic Context (2-3 min opening)
- Write an opinionated opening that connects the question to the company's specific strategic context (Revenue, Competitive dynamics, and why this matters *now*).

### Shift 2: Relationship Segmentation
- Segment users by their relationship to the **Core Capability** (e.g., "Power Users" vs. "Casual" is a 6/10; segmenting by "Trust Level" or "Relationship to AI Output" is a 10/10).

### Shift 3: Architectural Distinction (Model/Core vs. Application)
- Specify which parts of the solution require **Core/Model Layer** changes (longer dev cycle, high risk) vs. **Application Layer** changes (faster, product-owned). 

### Shift 4: Trust-by-Design
- Build safety, governance, and reliability into each individual solution—not as a section at the end. (e.g., "Verification loops for automated actions").

### Shift 5: Growth Flywheels (The "Math")
- Defend the proposal with specific levers: Activation, Retention, or Word-of-Mouth flywheels. Avoid hand-waving "engagement" metrics.

---

## Step 6 — Candidate Strategic Bridge

Generate **5 high-signal bullet points** that explicitly bridge the candidate's profile (`profile.md`) to the strategic challenges identified in the "5 Shifts".
- Focus on how their background in **API Governance, ISO 20022 standards, and Platform Lifecycle** makes them uniquely qualified to solve the "Shift 3" and "Shift 4" problems for this specific role.

---

## Step 7 — Interview question sets
 
Generate **3 to 5 high-signal questions** for each likely interviewer type (Hiring Manager, Engineering Lead, Product Leadership). These must be tailored to the specific findings from Step 2.
 
For each question, add a one-line coaching note in _italics_ beneath it:
> _What they're really testing: [underlying concern or signal they're looking for]_
 
---
 
## Step 8 — Save and present
 
Save the full output as a markdown file.
 
**Path:** `./.ai-rulez/skills/job-search/references/prep/YYYY-MM-DD-[company-slug]-[role-slug].md`
(Add `-comprehensive` to the filename if Deep Dive mode was used).
 
---
 
## Notes
 
- **Adaptive Depth:** Tailor research to company size. Don't hunt for a CEO's personal philosophy at a 50k-person conglomerate; find the BU's strategy deck.
- **Domain Agnostic:** Use generalized language (e.g., "The Domain", "Primary Stakeholders", "Core Platform") to ensure the skill works for any industry.
- **Honest Intelligence:** Prioritize clear-eyed analysis over a sales pitch.
- **Chaining:** The proposals are designed to feed the **proposal-builder** skill.

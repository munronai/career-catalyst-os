---
name: proposal-builder
description: >
  Rapidly prototypes and documents high-value proposals for job applications.
  Turns role-prep ideas into "Proof of Value" packages including research, PRDs,
  strategy, architecture, and agent reviews.
---

# Proposal Builder Skill

This skill is designed to help a candidate (the user) quickly build a compelling, well-researched proposal for a specific role. The goal is to demonstrate "value-add" and technical/strategic thinking in 3–4 hours, prioritizing high-signal output over perfection.

---

## Workspace Structure

The skill operates within a project-specific directory (e.g., `reports/proposals/[proposal-name]/`). Within that root, it maintains the following structure:

- `research/`: Tabulated data, competitive analysis, and discovery reports.
- `prds/`: Product Requirement Documents.
- `one-pagers/`: High-level summaries for quick reading.
- `strategy/`: Vision, strategy, and alignment with company goals.
- `technical-architecture/`: System designs, diagrams (text-based), and tech specs.
- `presentations/`: 5-slide McKinsey-style presentation decks (markdown-formatted).
- `user-stories/`: Granular delivery requirements.
- `reviews/`: Feedback reports from specialized agent profiles.

The default base directory is ./.ai-rulez/skills/job-search/references/reports/, but the user can specify a custom path when initiating the job search or proposal builder directly.  

---

## The Workflow

The skill follows a strict 7-phase execution cycle. Do not skip phases unless explicitly directed.

### Phase 1: Research & Discovery
**Goal:** Gather empirical or simulated data to support the "Problem" statement.
- Perform targeted searches for the specific pain point (e.g., "Lawhive pricing discrepancies", "legal tech intake friction").
- Synthesize findings into a tabulated report (CSV or Markdown Table) in the `research/` directory.
- Identify "Areas of Concern" where the proposal brings the most value.

### Phase 2: Requirements & One-Pager
**Goal:** Define *what* we are building and *why*.
- Create a **One-Pager** in `one-pagers/`: Focus on the Hook, the Problem, the Solution, and the Value.
- Create a **PRD** in `prds/`: Define Objectives, User Flows, Functional Requirements, and Success Metrics.

### Phase 3: Strategy & Vision
**Goal:** Align the proposal with the company's long-term goals.
- Create a **Strategy Document** in `strategy/`.
- **Constraint:** Clearly state all assumptions (e.g., "Assuming Lawhive's 2026 goal is 10x US user growth...").
- Define how this specific feature/project acts as a multiplier for the wider company strategy.

### Phase 4: Technical Architecture
**Goal:** Prove technical feasibility and "how" it works.
- Create a **Tech Spec** in `technical-architecture/`.
- Include data flow diagrams (Mermaid/ASCII), component breakdowns, and API designs.
- Focus on integration with existing company tech (e.g., Lawhive's "Lawrence" or `ai-rulez`).

### Phase 5: Presentation Deck
**Goal:** Create a pitch-ready summary.
- Create a **5-Slide Presentation** (Markdown) in `presentations/`.
- Slide 1: Title & The Value Hook.
- Slide 2: The Data-Backed Problem (from Research).
- Slide 3: The Solution & Strategy.
- Slide 4: Technical Implementation & "Why Me" (Candidate Advantage).
- Slide 5: Impact & Success Metrics.

### Phase 6: User Stories
**Goal:** Demonstrate readiness for delivery.
- Create a set of **User Stories** in `user-stories/` using the Gherkin format (Given/When/Then).
- Cover the "Happy Path" and critical edge cases.

### Phase 7: Agent Reviews
**Goal:** Simulate high-level stakeholder feedback.
- Execute three distinct "Agent Personas" to review the entire package:
    1. **Executive (CEO/CFO):** Focus on Vision, Strategy, ROI, and Brand.
    2. **Engineering (CTO/Lead):** Focus on Feasibility, Complexity, and Maintenance.
    3. **User Research (UX Lead):** Focus on User Friction, Empathy, and Clarity.
- Save each review as a markdown report in `reviews/`.

---

## Execution Principles

1. **Velocity First:** Aim for "good enough to prove the point." Do not spend more than 30 minutes on any single document.
2. **"Here is what I can do for you":** Every document must be written from the perspective of an incoming PM solving a problem for the employer.
3. **Traceability:** Ensure the Presentation Slide 2 links to the Research data, and the Tech Spec links to the PRD.
4. **AI-Native Delivery:** Explicitly use AI tools to generate data, diagrams, and stories to demonstrate your ability to deliver quickly.

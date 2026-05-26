# AI Product Management Workflow

> A plug-and-play Claude Code workflow that gives any product manager a full AI-powered product team — from problem validation through prototyping — orchestrated by a single intelligent router.

---

## What This Is

This repository is a **multi-agent AI product management system** built on top of [Claude Code](https://claude.ai/code). You drop your project context in, describe what you're working on, and the orchestrator routes your input to the right specialist agent automatically.

Each agent owns a specific PM discipline, maintains its own persistent memory, produces real artifacts (`.docx`, `.xlsx`, `.pptx`, Figma files), and communicates with peer agents when cross-stream work is triggered — so a PRD change automatically triggers test script updates, portfolio deck refreshes, and backlog re-prioritisation without you having to ask.

Think of it as hiring a fractional product team that never forgets context, never drops the ball on cross-functional alignment, and works at the speed of a prompt.

---

## Agent Landscape

```
                        ┌─────────────────────────────────┐
                        │        YOU  (Product Manager)    │
                        └────────────────┬────────────────┘
                                         │  any prompt
                                         ▼
                        ┌─────────────────────────────────┐
                        │        ORCHESTRATOR              │
                        │  (routes · tracks · advises)     │
                        └──┬───────┬────────┬──────┬──────┘
                           │       │        │      │
              ┌────────────┘  ┌────┘   ┌───┘  ┌──┘
              ▼               ▼        ▼      ▼
      ┌──────────────┐ ┌──────────┐ ┌──────┐ ┌──────────┐ ┌───────────┐
      │  STRATEGY    │ │  DOCS    │ │ TEST │ │ BACKLOG  │ │PROTOTYPE  │
      │  Agent 1     │ │  Agent 2 │ │ Agt 3│ │  Agent 4 │ │  Agent 5  │
      └──────────────┘ └──────────┘ └──────┘ └──────────┘ └───────────┘
```

| Agent | Stream | Owns |
|---|---|---|
| **Strategy** | 1 – Strategy | Problem validation, value props, solution ideation, product strategy doc, roadmap |
| **Documentation** | 1 – Strategy | PRD, KDD (Key Decision Document), gap analysis, success metrics |
| **Tester** | 2 – Quality | Test scripts (Excel), requirement traceability, coverage review |
| **Backlog Manager** | 3 – Execution | Portfolio backlog ledger, project backlog ledger, stakeholder deck (PPTX portfolio view) |
| **Prototype** | On-demand | Figma high-fidelity mockups and wireframes |

---

## Quick Start

### Prerequisites

- [Claude Code CLI](https://github.com/anthropics/claude-code) installed and authenticated
- A Claude account with API access
- Figma account (only required for the Prototype agent)

### Setup

```bash
# 1. Clone the repo
git clone https://github.com/<your-org>/ai-pm-workflow.git
cd ai-pm-workflow

# 2. Open Claude Code in this directory
claude

# 3. Just start talking — describe your project or problem statement
```

### Feeding a Project

**Option A — Blank slate (discovery mode)**
```
I'm starting a new project. The problem I'm trying to solve is [your problem statement].
```
The Strategy agent takes over, pressure-tests your problem, and kicks off the full discovery flow.

**Option B — Pre-defined backlog item (no discovery yet)**
```
I have a new project idea: [description]. No discovery needed yet — just add it to the portfolio backlog.
```
The Backlog Manager logs it in the portfolio backlog, prioritises it, and updates the stakeholder deck.

**Option C — Feature within an active project**
```
For [project name]: add [feature description] to the backlog.
```
The Backlog Manager logs it in the project-level backlog ledger.

---

## Directory Structure

```
ai-pm-workflow/
├── instructions.md                   ← you are here
├── CLAUDE.md                         ← Claude Code configuration & orchestrator rules
├── orchestrator.md                   ← routing logic, escalation rules, process advice
│
├── agents/
│   ├── strategy/
│   │   ├── agent.md                  ← strategy agent persona & responsibilities
│   │   └── skill.md                  ← skill trigger definitions
│   ├── documentation/
│   │   ├── agent.md
│   │   └── skill.md
│   ├── tester/
│   │   ├── agent.md
│   │   └── skill.md
│   ├── backlog/
│   │   ├── agent.md
│   │   └── skill.md
│   └── prototype/
│       ├── agent.md
│       └── skill.md
│
├── reference/                        ← drop any reference materials here
│   └── README.md
│
├── portfolio/
│   └── portfolio-backlog.md          ← all new/planned projects across their lifecycle
│
├── projects/                         ← one folder per active project
│   └── [project-name]/
│       ├── strategy/
│       │   └── product-strategy.docx
│       ├── prd/
│       │   ├── PRD_v1.0.docx
│       │   └── KDD.docx
│       ├── test-scripts/
│       │   └── test-scripts.xlsx
│       ├── backlog/
│       │   └── backlog-ledger.md     ← feature/task backlog within this project only
│       ├── session-notes/
│       │   └── YYYY-MM-DD.md         ← end-of-session handoff notes (one per session)
│       └── open-actions.md           ← live actions tracker, updated throughout sessions
│
└── memory/                           ← shared cross-agent memory (auto-managed)
    ├── MEMORY.md
    └── *.md
```

---

## Backlog Architecture

There are **two distinct backlog layers**. The Backlog Manager operates across both:

### 1. Portfolio Backlog (`portfolio/portfolio-backlog.md`)

Tracks **initiatives and new projects** across their full lifecycle — from raw idea through to shipped. This is org-wide and not tied to any single project. Projects move through these stages:

```
Idea → Discovery → In Progress → Shipped / Cancelled
```

This is the source of truth for **what we're building next** and feeds directly into the stakeholder PPTX deck.

### 2. Project Backlog (`projects/[name]/backlog/backlog-ledger.md`)

Tracks **features, stories, and tasks** scoped within a single active project. Only exists once a project has been kicked off (moved to Discovery or In Progress in the portfolio). This is intentionally kept **out of the PPTX deck** as it's too granular for stakeholder socialisation — it's an internal working document.

### Disambiguation Rule

When you say *"add this to the backlog"* the Backlog Manager will ask one clarifying question if the context is ambiguous: **"Is this a new initiative we haven't kicked off yet, or a scoped item within [current project]?"**

---

## Stakeholder Deck (PPTX)

The deck is a **portfolio-level document** — it shows leadership and stakeholders the full landscape of work, not internal task lists. It intentionally does **not** include project-level feature backlogs.

**Deck contents:**
1. **Year Strategy** — key themes and strategic bets
2. **Portfolio Roadmap** — timeline view of all initiatives (Idea → Shipped)
3. **In Progress** — deep-dive on actively running projects: status, owner, ETA, risks
4. **Portfolio Backlog** — prioritised list of upcoming initiatives with RICE scores and tentative dates
5. **Shipped** — recent completions and impact summary

McKinsey-style formatting: clean typography, structured infographics, consistent colour language.

---

## How the Workflow Operates

### Orchestrator Routing

Every prompt goes through the Orchestrator first. It reads your intent and dispatches to the right agent(s). You never need to address an agent directly.

| What you say | Who handles it |
|---|---|
| "I think the problem is that users can't find X" | Strategy agent (problem validation) |
| "Update the PRD — we're dropping feature Y" | Documentation agent → triggers Tester (script update) + Backlog Manager (portfolio ledger check) |
| "Prioritise the portfolio backlog" | Backlog Manager |
| "Add this new project idea to the backlog" | Backlog Manager (portfolio backlog) |
| "Add this feature to [project] backlog" | Backlog Manager (project backlog) |
| "Create a wireframe for the onboarding flow" | Prototype agent (Figma) |
| "What am I missing in my process?" | Orchestrator (process advisory) |

### Cross-Agent Cascade Rules

The following changes **always** trigger downstream agents automatically:

| Trigger | Downstream effect |
|---|---|
| PRD section added or changed | Tester reviews and updates test scripts |
| PRD section added or changed | Backlog Manager checks if portfolio items are affected |
| Strategy doc updated | Documentation agent syncs PRD context |
| New portfolio backlog item added | Backlog Manager re-prioritises and updates PPTX |
| Feature removed from PRD | Tester removes associated test cases |
| Project moved to In Progress | Documentation agent creates PRD + KDD; Tester creates test script workbook |
| Prototype created | Documentation agent captures design decisions in KDD |

### Session Continuity & Context Persistence

Context is never lost between sessions. Three layers work together to ensure every new session picks up exactly where the last one left off:

**Layer 1 — Session Handoff Notes** (`projects/[name]/session-notes/YYYY-MM-DD.md`)
The Documentation Agent produces a structured note at the end of every session covering what was discussed, decisions made, open questions, open actions, and where to pick up next. This is the first thing read at the start of the following session — takes seconds to load, no raw transcript noise.

**Layer 2 — Open Actions Tracker** (`projects/[name]/open-actions.md`)
A live, running list of outstanding actions updated throughout every session as actions are raised or resolved. Never wait until end of session — actions are captured in real time.

**Layer 3 — Memory System** (`memory/`)
Captures cross-project patterns, user preferences, and long-horizon decisions. Persists indefinitely across projects.

**To trigger a session wrap-up**, say: *"wrap up"*, *"end session"*, or *"let's stop here"* — the Documentation Agent will produce the handoff note and update the open actions tracker automatically.

**The Documentation Agent is always listening** — passively monitoring all conversation across every session, silently tracking decisions, actions, and unresolved questions regardless of which agent is the primary responder.

### Agent Memory & Learning

Every agent maintains memory files under `memory/`. Agents record decisions made and their rationale, patterns observed across projects, feedback corrections, and project-specific context. Memory persists across sessions — agents pick up exactly where you left off.

---

## Agent Responsibilities

### Agent 1 — Strategy

- Validates whether the problem you've described is real and worth solving
- Challenges weak problem statements with pointed, data-driven questions
- Defines the problem space: user pain points, impact, root cause
- Proposes and critiques solutions
- Builds and maintains `product-strategy.docx`
- Defines the product roadmap
- Leads requirement gathering

**Output:** `projects/[name]/strategy/product-strategy.docx`

---

### Agent 2 — Documentation

- Authors and maintains the **PRD**: executive summary, problem statement, goals, non-goals, user stories, functional requirements, non-functional requirements, dependencies, success metrics, open questions
- Authors and maintains the **KDD**: every significant product decision logged with context, options considered, decision made, and rationale
- Performs gap analysis — surfaces missing requirements, integration impacts, edge cases
- Defines success metrics and KPIs
- Proactively asks clarifying questions; never assumes on ambiguous requirements
- **Always listening** — passively monitors all conversation across every session, silently capturing decisions, actions, and unresolved questions
- Produces **session handoff notes** at end of every session so next session starts with full context in seconds
- Maintains the **open actions tracker** in real time throughout every session

**Outputs:** `projects/[name]/prd/PRD_vX.X.docx`, `projects/[name]/prd/KDD.docx`, `projects/[name]/session-notes/YYYY-MM-DD.md`, `projects/[name]/open-actions.md`

---

### Agent 3 — Tester

- Reads the PRD and generates a professional test script Excel workbook
- Columns: Test ID, Application Tested, Test Suite, Description, Pre-conditions, Test Steps, Expected Result, Actual Result, Status, Priority, Tested By, Date, Notes
- Colour-coded by status (Pass / Fail / Blocked / Not Started) and priority (Critical / High / Medium / Low)
- Summary tab: project overview, coverage stats, pass rate, open defects
- Automatically reconciles test cases when PRD changes
- Flags untestable requirements back to the Documentation agent

**Output:** `projects/[name]/test-scripts/test-scripts.xlsx`

---

### Agent 4 — Backlog Manager

- Maintains `portfolio/portfolio-backlog.md`: all initiatives across their lifecycle with owner, RICE score, status, and tentative dates
- Maintains `projects/[name]/backlog/backlog-ledger.md`: feature-level backlog within an active project
- Builds and updates the stakeholder PPTX (portfolio view only — no project-level feature backlogs)
- Prioritises using **RICE scoring** (Reach × Impact × Confidence ÷ Effort); asks targeted questions to calibrate
- Grooms both backlogs: removes completed items, flags blocked items, surfaces stale items

**Outputs:** `portfolio/portfolio-backlog.md`, `projects/[name]/backlog/backlog-ledger.md`, `portfolio/stakeholder-deck.pptx`

---

### Agent 5 — Prototype

*Invoked only on explicit prototyping requests.*

- Reads PRD, strategy doc, and reference materials before opening Figma
- Creates high-fidelity mockups and wireframes in Figma
- Follows brand guidelines if present in `reference/`
- Hands off design decisions to Documentation agent for KDD logging
- Iterates based on feedback

---

## Process Advisory

Ask the Orchestrator process questions at any time:

```
"What am I missing in my process?"
"How can I improve the way we're running this project?"
"Are we skipping any PM best practices?"
"What should I be doing before we go to engineering?"
```

The Orchestrator benchmarks your current state against PM best practices and gives you concrete, prioritised recommendations.

---

## Reference Materials

Drop any supporting files into `reference/`:
- Brand guidelines
- Technical architecture docs
- Competitor analyses
- User research reports
- Design system specs
- API documentation

All agents check `reference/` when context is relevant to their task.

---

## Contributing / Extending

To add a new agent:

1. Create `agents/[name]/agent.md` — define the agent's persona, responsibilities, input triggers, outputs, and cascade rules
2. Create `agents/[name]/skill.md` — define the skill trigger keywords and routing conditions
3. Register the agent in `CLAUDE.md` under the routing table
4. Update `instructions.md` with the new agent's section

To modify routing logic, edit `orchestrator.md`.

---

## License

MIT — use this freely, fork it, extend it.

---

*Built with [Claude Code](https://claude.ai/code) by Anthropic.*

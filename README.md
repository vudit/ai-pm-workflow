# 🤖 AI PM Workflow

> A plug-and-play Claude Code workflow that gives any product manager a full AI-powered product team — from problem validation through prototyping — orchestrated by a single intelligent router.

---

## 💡 What This Is

This repository is a **multi-agent AI product management system** built on top of [Claude Code](https://claude.ai/code). You drop your project context in, describe what you're working on, and the orchestrator routes your input to the right specialist agent automatically.

Each agent owns a specific PM discipline, maintains its own persistent memory, produces real artifacts (`.docx`, `.xlsx`, `.pptx`, Figma files), and communicates with peer agents when cross-stream work is triggered — so a PRD change automatically triggers test script updates, portfolio deck refreshes, and backlog re-prioritisation without you having to ask.

Think of it as hiring a fractional product team that never forgets context, never drops the ball on cross-functional alignment, and works at the speed of a prompt.

---

## 🗺️ Agent Landscape

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

| Agent | Owns |
|---|---|
| **Strategy** | Problem validation, value props, solution ideation, product strategy doc, roadmap |
| **Documentation** | PRD, KDD (Key Decision Document), gap analysis, success metrics, session handoff notes |
| **Tester** | Test scripts (Excel), requirement traceability, coverage review |
| **Backlog Manager** | Portfolio backlog ledger, project backlog ledger, stakeholder deck (PPTX) |
| **Prototype** | Figma high-fidelity mockups and wireframes (on-demand) |

---

## 🚀 Quick Start

### Prerequisites

- [Claude Code](https://github.com/anthropics/claude-code) installed and authenticated
- A Claude account with API access
- Figma account (only required for the Prototype agent)

### Setup

```bash
# 1. Clone the repo
git clone https://github.com/<your-username>/ai-pm-workflow.git
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

## 📁 Directory Structure

```
ai-pm-workflow/
├── README.md
├── CLAUDE.md                         ← Claude Code configuration & orchestrator rules
├── orchestrator.md                   ← routing logic, escalation rules, process advice
│
├── agents/
│   ├── strategy/
│   │   ├── agent.md                  ← agent persona & responsibilities
│   │   └── skill.md                  ← skill trigger definitions
│   ├── documentation/
│   ├── tester/
│   ├── backlog/
│   └── prototype/
│
├── reference/                        ← drop any reference materials here
│
├── portfolio/
│   └── portfolio-backlog.md          ← all initiatives across their lifecycle
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
│       │   └── backlog-ledger.md
│       ├── session-notes/
│       │   └── YYYY-MM-DD.md
│       └── open-actions.md
│
└── memory/                           ← shared cross-agent memory (auto-managed)
    ├── MEMORY.md
    └── *.md
```

---

## ⚙️ How the Workflow Operates

### Orchestrator Routing

Every prompt goes through the Orchestrator first. It reads your intent and dispatches to the right agent. You never need to address an agent directly.

| What you say | Who handles it |
|---|---|
| "I think the problem is that users can't find X" | Strategy agent |
| "Update the PRD — we're dropping feature Y" | Documentation → triggers Tester + Backlog Manager |
| "Prioritise the portfolio backlog" | Backlog Manager |
| "Add this new project idea to the backlog" | Backlog Manager (portfolio backlog) |
| "Add this feature to [project] backlog" | Backlog Manager (project backlog) |
| "Create a wireframe for the onboarding flow" | Prototype agent (Figma) |
| "What am I missing in my process?" | Orchestrator (process advisory) |

### Cross-Agent Cascade Rules

The following changes **always** trigger downstream agents automatically — no need to ask:

| Trigger | Downstream effect |
|---|---|
| PRD section added or changed | Tester reviews and updates test scripts |
| PRD section added or changed | Backlog Manager checks if portfolio items are affected |
| Strategy doc updated | Documentation agent syncs PRD context |
| New portfolio backlog item added | Backlog Manager re-prioritises and updates PPTX |
| Feature removed from PRD | Tester removes associated test cases |
| Project moved to In Progress | Documentation creates PRD + KDD; Tester creates test script workbook |
| Prototype created | Documentation agent captures design decisions in KDD |

### Session Continuity

Context is never lost between sessions. Three layers work together:

**Layer 1 — Session Handoff Notes** (`projects/[name]/session-notes/YYYY-MM-DD.md`)
The Documentation Agent produces a structured note at the end of every session: decisions made, open questions, open actions, and where to pick up next.

**Layer 2 — Open Actions Tracker** (`projects/[name]/open-actions.md`)
A live, running list of outstanding actions updated in real time throughout every session.

**Layer 3 — Memory System** (`memory/`)
Captures cross-project patterns, user preferences, and long-horizon decisions. Persists indefinitely.

To trigger a session wrap-up, say: *"wrap up"*, *"end session"*, or *"let's stop here"*.

---

## 📋 Backlog Architecture

There are two distinct backlog layers — the Backlog Manager operates across both.

### Portfolio Backlog (`portfolio/portfolio-backlog.md`)

Tracks initiatives and new projects across their full lifecycle. This is org-wide, not tied to any single project, and feeds directly into the stakeholder PPTX deck.

```
Idea → Discovery → In Progress → Shipped / Cancelled
```

### Project Backlog (`projects/[name]/backlog/backlog-ledger.md`)

Tracks features, stories, and tasks scoped within a single active project. Intentionally kept out of the PPTX deck — it's too granular for stakeholder socialisation.

When you say *"add this to the backlog"* without context, the Backlog Manager will ask: **"Is this a new initiative we haven't kicked off yet, or a scoped item within [current project]?"**

---

## 📊 Stakeholder Deck

The PPTX deck is a **portfolio-level document** — it shows leadership the full landscape of work, not internal task lists.

**Deck contents:**
1. **Year Strategy** — key themes and strategic bets
2. **Portfolio Roadmap** — timeline view of all initiatives
3. **In Progress** — deep-dive on active projects: status, owner, ETA, risks
4. **Portfolio Backlog** — prioritised list with RICE scores and tentative dates
5. **Shipped** — recent completions and impact summary

---

## 🧠 Agent Responsibilities

### Strategy Agent

- Validates whether the problem is real and worth solving
- Challenges weak problem statements with pointed, data-driven questions
- Proposes and critiques solutions
- Builds and maintains `product-strategy.docx` and the product roadmap

**Output:** `projects/[name]/strategy/product-strategy.docx`

### Documentation Agent

- Authors and maintains the PRD and KDD
- Performs gap analysis — surfaces missing requirements, integration impacts, edge cases
- Passively monitors all conversation, silently capturing decisions and open questions
- Produces session handoff notes and maintains the open actions tracker in real time
- **Connects to your existing tools** — asks which tools to sync when a PRD is initiated; keeps docs in sync on every update

**Outputs:** `PRD_vX.X.docx`, `KDD.docx`, `session-notes/YYYY-MM-DD.md`, `open-actions.md`, `integrations.md`

### Tester Agent

- Reads the PRD and generates a professional test script Excel workbook
- Columns: Test ID, Application Tested, Test Suite, Description, Pre-conditions, Test Steps, Expected Result, Actual Result, Status, Priority, Tested By, Date, Notes
- Colour-coded by status (Pass / Fail / Blocked / Not Started) and priority (Critical / High / Medium / Low)
- Automatically reconciles test cases when the PRD changes

**Output:** `projects/[name]/test-scripts/test-scripts.xlsx`

### Backlog Manager Agent

- Maintains both the portfolio and project-level backlogs
- Builds and updates the stakeholder PPTX
- Prioritises using **RICE scoring** (Reach × Impact × Confidence ÷ Effort)
- Grooms both backlogs: removes completed items, flags blocked items, surfaces stale ones

**Outputs:** `portfolio-backlog.md`, `backlog-ledger.md`, `stakeholder-deck.pptx`

### Prototype Agent *(on-demand)*

- Reads PRD, strategy doc, and reference materials before opening Figma
- Creates high-fidelity mockups and wireframes
- Follows brand guidelines if present in `reference/`
- Hands off design decisions to Documentation agent for KDD logging

---

## 🎯 Process Advisory

Ask the Orchestrator process questions at any time:

```
"What am I missing in my process?"
"How can I improve the way we're running this project?"
"Are we skipping any PM best practices?"
"What should I be doing before we go to engineering?"
```

The Orchestrator benchmarks your current state against PM best practices (SVPG, Shape Up, dual-track agile) and returns concrete, prioritised recommendations — naming specific gaps, why they matter, and what to do first.

---

## 🔌 Tool Integrations

When a PRD is first initiated, the Documentation Agent asks:

> *"Would you like to sync this project's docs to an external tool?"*

| Tool | What gets synced |
|---|---|
| **Confluence** | PRD + KDD pages, kept in sync on every update |
| **JIRA** | Epics and stories created from functional requirements |
| **Notion** | PRD + KDD synced as Notion pages |
| **Linear** | Requirements mapped to Linear issues |
| **Other** | Anything with an API — provide connection details |

Credentials are stored as environment variable references — never as raw values. Integration config lives in `projects/[name]/integrations.md`.

---

## 📚 Reference Materials

Drop any supporting files into `reference/`:

- Brand guidelines
- Technical architecture docs
- Competitor analyses
- User research reports
- Design system specs
- API documentation

All agents check `reference/` when the context is relevant to their task.

---

## 🔧 Extending the Workflow

To add a new agent:

1. Create `agents/[name]/agent.md` — define the persona, responsibilities, input triggers, outputs, and cascade rules
2. Create `agents/[name]/skill.md` — define trigger keywords and routing conditions
3. Register the agent in `CLAUDE.md` under the routing table
4. Update `orchestrator.md` with any new cascade rules

To modify routing logic, edit `orchestrator.md`.

---

## 📄 License

MIT — use this freely, fork it, extend it.

---

*Built with ❤️ and [Claude Code](https://claude.ai/code) by Anthropic.*

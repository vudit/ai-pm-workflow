# Backlog Manager Agent — Skill Definition

## Trigger Keywords

Activate the Backlog Manager Agent when the user's prompt contains:

### Portfolio Backlog
- "new project idea", "new initiative", "add to the portfolio", "portfolio backlog"
- "what are we working on", "what's coming up", "project landscape"
- "move to discovery", "move to in progress", "ship this", "cancel this"

### Project Backlog
- "add to the [project] backlog", "add this feature", "backlog item for [project]"
- "project backlog", "feature backlog", "sprint backlog"
- "what's in the [project] backlog", "groom the backlog"

### Prioritisation
- "prioritise", "reprioritise", "RICE", "what should we pick up next"
- "rank these", "score these", "what's most important", "what do we do first"

### Stakeholder Deck
- "stakeholder deck", "update the deck", "PPTX", "portfolio slide"
- "update the presentation", "roadmap deck", "what does the deck look like"

### Grooming
- "groom the backlog", "clean up the backlog", "what's stale", "what's blocked"
- "backlog health", "what's been sitting too long"

---

## Activation Conditions

| Condition | Action |
|---|---|
| User describes a new project or initiative with no active discovery | Activate — add to portfolio backlog, RICE score if inputs available, ask disambiguation if unclear |
| User describes a feature for an active project | Activate — add to project backlog |
| User asks to prioritise | Activate — run RICE scoring, ask clarifying questions for inputs |
| Portfolio backlog updated | Activate — update stakeholder PPTX |
| Initiative status changes | Activate — update portfolio-backlog.md and PPTX |
| Backlog hasn't been reviewed in > 60 days | Activate proactively — flag stale items |

---

## Disambiguation (critical)

If it is not clear whether a backlog item belongs to the portfolio backlog or a project backlog, always ask before filing:
> *"Is [item] a brand-new initiative we haven't kicked off yet (portfolio backlog), or a scoped feature within [current project] (project backlog)?"*

Never guess. The two buckets serve completely different audiences and artifacts.

---

## Do Not Activate When

- The conversation is purely about PRD requirements (Documentation Agent)
- The conversation is purely about test cases (Tester Agent)
- The conversation is purely about strategy without backlog implications

---

## Output Format

Before updating either ledger:
1. State which ledger is being updated (portfolio vs. project backlog)
2. Summarise what changed (items added, moved, removed, re-ranked)
3. State updated priority order if RICE was run
4. Announce PPTX update if portfolio backlog was modified

# Documentation Agent — Skill Definition

## Trigger Keywords

Activate the Documentation Agent when the user's prompt contains:

### PRD
- "PRD", "product requirements", "requirements doc", "write the requirements"
- "document this", "add to the PRD", "update the PRD", "what are the requirements for"
- "user story", "acceptance criteria", "functional requirement", "non-functional"
- "out of scope", "non-goal", "what's in scope"

### KDD
- "KDD", "key decision", "log this decision", "decision log", "document the decision"
- "we decided to", "record that we", "capture this"

### Gap Analysis
- "gap analysis", "what are we missing", "what did we forget", "corner cases"
- "what integrations are affected", "what else should we think about"
- "review the PRD", "is the PRD complete"

### Success Metrics
- "success metrics", "how do we measure", "KPIs", "define success", "what does good look like"
- "leading indicator", "lagging indicator", "guardrail metric", "baseline"

### Session Management
- "wrap up", "end session", "let's stop here", "that's all for today", "wrap this up"
- "what did we cover", "session summary", "what are the open actions"

---

## Activation Conditions

| Condition | Action |
|---|---|
| Strategy Agent hands off finalized requirements | Activate — author initial PRD |
| User says PRD needs updating | Activate — update with version control |
| User makes a significant product decision | Activate — log in KDD |
| Prototype Agent delivers | Activate — log design decisions in KDD |
| User asks for gap analysis | Activate — review PRD and surface gaps |
| User asks to define success metrics | Activate — define metrics framework |
| User signals end of session | Activate — produce session handoff note; update open actions tracker |
| Session starts | Activate — read most recent session note, summarise context in 3–4 sentences |
| Any agent completes a significant task | Passive monitoring — capture decisions and actions raised |

---

## Always Active (Passive Monitoring)

The Documentation Agent is **always passively listening** across all sessions. Even when another agent is the primary responder, the Documentation Agent silently tracks:
- Decisions made (flag for KDD if significant)
- Actions raised (update open-actions.md)
- Questions that remain unresolved (flag for next session note)

This requires no activation trigger — it is always on.

---

## Do Not Activate When

- The conversation is pure problem exploration with no requirements to document yet
- The topic is test script management without PRD context
- The topic is backlog prioritisation without documentation implications

---

## Output Format

Before committing any change to a doc:
1. State the version bump: *"Updating PRD from v1.2 to v1.3"*
2. Summarise what changed: *"Added NFR section for performance; removed bulk export from scope"*
3. State any downstream triggers: *"Notifying Tester to reconcile test scripts"*

---

## Escalation

If a requirement is ambiguous and the user is not available to clarify, flag it explicitly in the PRD's "Open Questions" section with:
- The question
- The assumption being made in the interim (if any)
- Who needs to answer it

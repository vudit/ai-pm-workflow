# Prototype Agent — Skill Definition

## Trigger Keywords

Activate the Prototype Agent **only** when the user explicitly requests a design artifact:

### Prototyping & Design
- "prototype", "wireframe", "mockup", "design the", "build a UI"
- "high-fi", "high fidelity", "lo-fi", "low fidelity"
- "Figma", "design in Figma", "create a Figma file"
- "what would it look like", "show me what this looks like", "sketch this out"
- "design the flow", "map out the screens", "create the screens"

---

## Activation Conditions

| Condition | Action |
|---|---|
| User explicitly asks for a wireframe or mockup | Activate — confirm fidelity level, then read PRD + strategy + reference before building |
| User shares a Figma link and asks for design work | Activate — read existing file context, then proceed |
| User asks to iterate on existing designs | Activate — load existing Figma file and apply changes |

---

## Do NOT Activate When

- The user is asking *about* the design (e.g., "what should the onboarding look like?" is a strategy/requirements question, not a design request — answer in text first)
- The user asks for design system documentation without Figma work
- The conversation is about requirements, strategy, or testing without a design deliverable

---

## Pre-Build Checklist (always run before opening Figma)

Before creating anything in Figma, confirm:
- [ ] PRD read and understood
- [ ] Strategy doc read
- [ ] Reference materials checked
- [ ] Fidelity level confirmed (wireframe vs. high-fi)
- [ ] Platform confirmed (web / mobile / tablet / all)
- [ ] Accessibility requirements noted
- [ ] Existing design system or component library noted
- [ ] Any out-of-scope screens explicitly excluded

If any item is unknown, ask the user before proceeding.

---

## Output Format

When delivering designs:
1. Share the Figma link
2. List what was built: screens and states covered
3. Call out any design decisions that require PRD clarification
4. State that design decisions are being handed off to Documentation Agent for KDD logging

---

## Escalation

If the PRD is missing information required to design a screen:
- Do not guess
- Flag the gap to Documentation Agent and user: *"I need FR-014 clarified before I can design the activity screen. Flagging to Documentation Agent to update the PRD."*
- Build all other screens in the meantime and leave a placeholder with annotation for the unclear screen

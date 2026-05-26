# Tester Agent — Skill Definition

## Trigger Keywords

Activate the Tester Agent when the user's prompt contains:

### Test Scripts
- "test script", "test case", "test cases", "write tests", "create test plan"
- "generate test cases", "test coverage", "what tests do we need"

### Test Execution
- "mark as passed", "mark as failed", "update test status", "test result"
- "TC-[number]", "test ID", "run the tests"

### QA / Quality
- "QA", "quality assurance", "defect", "bug", "test the PRD"
- "what's covered", "coverage report", "are we ready to test"

### PRD Change (cascade)
- Triggered automatically by Orchestrator when PRD is updated — no keyword needed from user

---

## Activation Conditions

| Condition | Action |
|---|---|
| New project moves to In Progress | Activate — create initial test script workbook from PRD |
| PRD version updated | Activate — reconcile test scripts (cascade) |
| User asks to update test status | Activate — update workbook |
| User asks for coverage analysis | Activate — run coverage review |
| User asks to generate tests for new feature | Activate — write new test cases and add to workbook |
| Feature removed from PRD | Activate — remove associated test cases |

---

## Do Not Activate When

- The conversation is about PRD structure or requirements writing (Documentation Agent)
- The conversation is about strategy or problem validation
- The conversation is about backlog or prioritisation without QA implications

---

## Output Format

Always report:
1. What was done (e.g., "Added 6 test cases, removed 2, updated 1")
2. Current stats (Total / Pass / Fail / Blocked / Not Started)
3. Any flags (untestable requirements, coverage gaps)

---

## Escalation

If a requirement cannot be translated to a testable test case:
- Do not skip it
- Flag it to Documentation Agent: *"Requirement FR-012 ('system should be fast') is not testable as written. Recommend adding: 'API response time must be < 200ms for 95th percentile under 100 concurrent users.'"*

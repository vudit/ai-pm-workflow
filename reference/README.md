# Reference Materials

Drop any supporting materials here. All agents check this folder when relevant to their task.

## What to put here

| File type | Examples |
|---|---|
| Brand guidelines | `brand-guidelines.pdf`, `design-tokens.json` |
| Technical architecture | `system-architecture.pdf`, `api-spec.yaml` |
| User research | `user-research-report.pdf`, `survey-results.xlsx` |
| Competitive analysis | `competitor-analysis.docx` |
| Design system | `design-system.fig`, `component-library.pdf` |
| Org context | `team-structure.md`, `tech-stack.md` |
| Any other project context | Specs, briefs, stakeholder notes |

## Naming convention

Use descriptive, lowercase, hyphenated filenames:
```
brand-guidelines-2025.pdf
api-architecture-overview.pdf
user-research-q1-2025.pdf
```

## How agents use these files

- **Strategy Agent**: reads for market context, user research, competitive landscape
- **Documentation Agent**: reads for integration specs, technical constraints, existing system context
- **Tester Agent**: reads for technical specs that affect test pre-conditions
- **Backlog Manager**: reads for strategic context when scoring RICE
- **Prototype Agent**: reads for brand guidelines, design system, and component library — mandatory before building in Figma

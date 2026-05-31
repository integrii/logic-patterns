---
name: "make-a-plan"
description: "Use when a task requires an implementation plan: repeatedly add major missing details, then simplify while preserving required outcomes and critical fixes."
---

# Planning

Use this skill when producing an implementation plan for a task.

Use this in conjunction with `development-patterns:plan-checklists` to persist, track, and lifecycle-checklist the plan you produce.

Required skills:
- `development-patterns:plan-checklists` for plan file location, checklist structure, and closure/move rules.

Workflow:
1. Draft an initial plan.
2. Ask/verify: “what major detail is missing from this plan?”
3. If any major missing detail is found:
   - add it to the plan
   - return to step 2
4. Repeat step 2–3 until no major details are missing.
5. When no major details are missing, run a simplification pass:
   - ask/verify: “Is there a way we can make the plan significantly simpler without sacrificing any of the desired outcomes or critical fixes?”
6. If significant simplification is possible:
   - apply the simplification(s)
   - return to step 5
7. Repeat simplification passes until no significant simplifications are found.
8. Finalize the plan only when both loops have reached no further changes.

Loop rules:
- “Major detail” means one of: missing dependencies, missing validation/test points, missing failure paths, missing rollback/observability/ownership, or missing contract/spec alignment.
- “Significant simplification” means reducing complexity while preserving:
  - intended outcomes
  - critical fixes
  - correctness and safety

Report format (recommended):
- Current plan version
- Newly added missing details from step 2
- Simplifications applied in each pass
- Remaining risks and why they were kept

---
name: "implement-plan"
description: "Execute an active implementation plan with scoped validation, review, final gates, and review-ready closure. Use when the user invokes `development-patterns:implement-plan`, `$development-patterns:implement-plan`, or asks to implement, continue, or finish a plan checklist."
---

# Implement Plan

Execute the active plan end-to-end. This coordinating skill does not replace applicable repository or domain skills. It authorizes bounded subagents unless the user requests local-only work or another instruction requires approval.

## Completion guard

Read this section before changing code.

- Do not complete, archive, or move work to review while a required gate is unchecked.
- Review evidence must cover the exact final diff. A material change after review requires fresh review evidence.
- A review timeout is not a clean result. Record the real error or blocker.

Add this ledger to the active plan. It is the single ready-for-review authority.

```markdown
## Completion gate

- [ ] Targeted validation is green: <commands and applicability>
- [ ] Required contract and regression coverage is complete: <evidence or rationale>
- [ ] Local review is clean: <scope, evidence>
- [ ] Independent review is clean or not required: <scope, evidence or rationale>
- [ ] Final build and applicable acceptance gates are green: <commands or rationale>
- [ ] Tracker is in the required review state, if used: <link or rationale>
```

After all preceding gates are green, archive the plan and complete the goal.

## 1. Scope once

- Load `development-patterns:plan-checklists`, every skill the affected surfaces require, and only the relevant project guidance, specifications, and design decisions.
- Create or reuse the plan-linked tracker when the project uses one.
- Use the plan's implementation points. Revise it only when repository evidence shows a smaller or safer design is needed. When a revision materially changes the selected design, scope, or acceptance criteria, update those plan sections and re-evaluate the decision before continuing.
- Select the canonical path and the observable contract for each slice. Apply the repository's required regression matrix when relevant.
- Choose final acceptance gates from the changed surface. UI, end-to-end, integration, and deployed-environment validation are required only when the plan or an applicable authority requires them.

## 2. Implement and validate slices

For each independently testable slice:

1. Implement the plan's selected canonical path. Keep minor variants as logic gates. Remove superseded behavior when safe.
2. Add or update the smallest tests that prove the slice's observable contract. For a bug fix, add a regression before the fix when practical.
3. Run focused validation and inspect authoritative readback for mutations.
4. Record material evidence in the ledger. Do not repeat generic discovery or run final gates until all slice checks are green.

Use an independent test author or subagent only when it materially improves confidence or delivery time. Perform UI rendering, contract-corpus checks, and integration or deployed-environment validation with the slice that requires them.

## 3. Stabilize the final diff

1. Load and run `$logic-patterns:gaslight-loop` locally against the completed diff.
2. For material code, contract, configuration, persistence, security, or cross-service changes, load `$logic-patterns:adversary-loop` and obtain a fresh independent review of that diff. Require concrete findings and review the design for unnecessary paths, state, configuration, and abstractions.
3. Record each review receipt with its scope, result, findings, fixes, and focused validation. For a non-material change, record why independent review is not required.
4. If a review finds a significant issue, fix it, rerun focused validation, repeat local review for the fix, then obtain a fresh independent review when one is required.

## 4. Close

- Confirm the ledger describes the exact final diff.
- Run the final build and the acceptance gates selected during scoping. Record an unavailable required gate with its reason.
- If a gate changes the diff, return to stabilization.
- Update the tracker when the project uses one, then use `development-patterns:plan-checklists` to archive the completed plan and complete the goal.
- Report the implementation points, validation, review receipts, tracker state when applicable, residual risks, and unavailable gates.

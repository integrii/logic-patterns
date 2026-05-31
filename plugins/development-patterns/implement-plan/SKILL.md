---
name: "implement-plan"
description: "Use when the user invokes `development-patterns:implement-plan`, `$development-patterns:implement-plan`, or asks to implement, continue, or finish an existing plan checklist end-to-end. Executes the plan through code changes, required SPEC.md alignment, GPT-5.5 medium implementation subagents, targeted validation, `$logic-patterns:adversary-loop`/`$logic-patterns:gaslight-loop` review loops, final build/e2e gates, and `$development-patterns:plan-checklists` archival."
---

# Implement Plan

Use this skill as the single entry point for executing an existing implementation plan.

This skill coordinates execution. It does not replace the domain skills that govern the code being changed.

Using this skill authorizes the workflow subagents described below unless the user explicitly asks for local-only implementation. If the active instruction set requires separate delegation permission and the user has not authorized this skill or subagents, ask before implementing.

## Required Skills

Load every applicable skill before implementation:
- `development-patterns:plan-checklists` for plan state and archival
- `development-patterns:spec-driven-development` for every affected `SPEC.md`
- `development-patterns:centralized-fix-selection` when choosing local vs shared fixes
- `logic-patterns:adversary-loop` for the post-implementation review loop
- `logic-patterns:gaslight-loop` for local iterative self-review and bug-fix verification

If two or more local plugin skills need non-trivial coordination, coordinate dependencies manually before execution.

## Implementation Workflow

1. Read the plan, repo `AGENTS.md`, relevant `SPEC.md`, and relevant `ADR.md` when decisions constrain the work.
2. Start by creating or reusing one tracker issue for this plan execution.
   - If this is a new plan execution, create one issue now, link to the plan, and describe the overall feature/fix at a high level.
   - Set the issue status to `In Progress`.
   - If continuing an existing run, update the existing plan-linked issue to `In Progress` instead of creating a duplicate.
   - Record the issue link in the plan checklist so there is a single source of truth for state.
3. Identify the touched behavior surfaces: routes, DTOs, persistence, events, commands, manifests, pages, packages, and tests.
4. Perform a `SPEC.md` alignment checkpoint for every affected repository. Update the relevant `SPEC.md` files whenever desired behavior, contracts, routes, DTOs, UI, persistence, integration behavior, or package boundaries change. If no spec edit is needed, record the explicit reason in the plan or final report.
5. Split the plan into concrete implementation slices with disjoint write ownership where practical.
6. Use GPT-5.5 medium worker subagents to write code for bounded slices. Tell each worker:
   - it is not alone in the codebase
   - the files or modules it owns
   - not to revert edits made by others
   - to keep changes simple, concrete, and spec-aligned
   - to run the smallest relevant validation it can
   - to report changed paths and validation results
7. Integrate worker results locally, resolve conflicts, and keep the plan checklist current.
8. Prefer targeted validation during implementation:
   - targeted Go tests for touched packages
   - targeted Playwright tests for changed browser flows
   - focused type checks, linters, or route tests when they are the smallest useful signal
9. Do not run `just build` or `just e2e` until the plan is fully implemented and targeted validation is passing or any failures are understood.

## Review Loop

After plan implementation is complete and before final build/e2e gates, run iterative review:

1. Use `$logic-patterns:adversary-loop`.
2. Use `$logic-patterns:gaslight-loop`.
3. Spawn a fresh GPT-5.5 high reviewer subagent for each adversary-loop pass.
4. Review the current diff, completed plan, relevant `SPEC.md` files, and targeted validation results.
5. Fix every significant valid finding locally.
6. Re-run the smallest relevant validation for each fix.
7. Repeat adversary-loop review with a fresh reviewer until the latest pass has no significant findings.
8. Continue gaslight-style local checks as needed while iterating on findings.

Significant findings include correctness bugs, incomplete plan items, spec drift, behavior regressions, weak validation for risky changes, and unnecessary complexity that hides risk.

## Final Gates

Only after the plan is fully implemented and the adversary-loop/gaslight-loop loops are clean:

1. Run `just agent-build` when available; otherwise run `just build` when available.
2. Run `just agent-e2e` when available; otherwise run `just e2e` when available.
3. If either gate fails, fix the first concrete failure with the smallest targeted validation loop, then return to the final gate.
4. Do not run another adversary-loop pass after successful e2e unless code or release configuration changed after e2e.

## Plan Closure

When implementation, adversary-loop review, and final gates are complete:

1. Use `development-patterns:plan-checklists`.
2. Check off every addressed plan item.
3. Move the associated tracker issue to a review/completion state appropriate for the repo's policy; do not mark it fully complete unless the user explicitly approves.
4. Archive the plan according to the active repository plan policy immediately after moving the issue to `In Review`, in the same closure step.
5. Report validation status, adversary-loop/gaslight-loop loop results, issue completion state, and any remaining risks.

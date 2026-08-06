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

Keep implementation and test authorship separate. An agent that writes or edits production code for a behavior must not write or edit the automated tests that validate that behavior. Assign all test creation and test updates to a different subagent. The coordinating agent may integrate both changes, but must not act as both the implementer and test author.

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
   - not to create or modify automated tests for the behavior it implements
   - to run the smallest relevant validation it can
   - to report changed paths and validation results
7. Assign automated test creation and updates for each implementation slice to a separate test-author subagent that did not implement that slice. Tell each test author:
   - the plan's expected behavior, acceptance criteria, and relevant public contracts
   - the test files or modules it owns
   - not to modify production code
   - to derive the test cases independently and report any behavior mismatch to the implementer
   - to run the smallest relevant test command and report changed paths and results
8. Integrate implementation and test-author results locally, resolve conflicts, and keep the plan checklist current.
9. Prefer targeted validation during implementation:
   - targeted Go tests for touched packages
   - targeted Playwright tests for changed browser flows
   - focused type checks, linters, or route tests when they are the smallest useful signal
10. Do not run `just build` or `just e2e` until the plan is fully implemented and targeted validation is passing or any failures are understood.

## Batch LAN Validation

When the plan includes LAN validation, especially UX or visual review, use a batch-first discovery loop:

1. Run one candidate in a session-scoped Golden Stack. Use port-forwarding to open the UI and log in with an ephemeral account for validation. Inspect every reachable required state before editing again. Include all planned steps, branches, success and error states, desktop and 390px layouts, keyboard or accessibility behavior, and the primary and secondary actions.
2. Keep a concrete finding ledger with the state, evidence, severity, user impact, and proposed fix. Continue the discovery pass after the first finding. Do not stop validation to fix an isolated issue when other required states remain reachable.
3. Batch all significant findings from that candidate into one coherent implementation slice. Fix related UI, behavior, copy, and test coverage together when they share the same flow or release scope.
4. Run focused checks on the batch, then build, run the required LAN gate, and perform one new visual discovery pass. Do not ship a new candidate for each individual cosmetic or UX finding.
5. Repeat the batch cycle until the finding ledger has no significant unresolved issues. Record non-critical findings that are intentionally deferred and why.

## UX Quality Gate

Run this gate for every plan that changes a user-facing UI. Run it after implementation and targeted tests, before the adversarial review loop.

Run the gate with a 5.6 Sol medium reviewer, a 5.6 Terra planner, and a 5.6 Luna implementer. The Sol reviewer evaluates the rendered result and acceptance criteria. The Terra planner identifies required UX corrections and validation. The Luna implementer applies the agreed fixes and reruns the smallest relevant UI check.

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

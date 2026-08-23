---
name: "implement-plan"
description: "Execute an implementation plan through independent tests, adversary and gaslight review loops, final gates, and review-ready closure. Use when the user invokes `development-patterns:implement-plan`, `$development-patterns:implement-plan`, or asks to implement, continue, or finish a plan checklist."
---

# Implement Plan

Execute the active plan end-to-end. This coordinating skill does not replace applicable domain skills. It authorizes bounded subagents unless the user requests local-only work or another instruction requires approval.

## Completion guard

Read this section before changing code. It is a hard stop, not advice.

- Never say the implementation is complete, call `update_goal(...complete)`, archive the plan, or move the tracker to `In Review` while any required completion-gate box is unchecked.
- Passing tests, a clean diff, a source scan, or a single review is not review-loop evidence.
- A review receipt is required for the final diff: reviewer or loop identity, scope, result, findings, fixes, and focused validation.
- Any code, test, documentation, configuration, or release change after a clean review invalidates that review receipt. Run fresh adversary and gaslight passes after the change.
- A polling timeout is not a clean review. Wait for a running reviewer for up to 15 minutes. Record a real terminal error or external blocker separately.
- Do not require a separate user approval after the requested implementation and every completion gate are satisfied. Archive the plan and mark the goal complete in the same closure step.

Immediately add this compact ledger to the active plan. Keep it current.

```markdown
## Completion gate

- [ ] Independent tests are green: <commands or rationale>
- [ ] HTTP endpoint regression matrix is complete: <unit, contract, authorization, validation, idempotency, and handler-chain evidence or applicability rationale>
- [ ] Adversary pass <n> is clean: <reviewer, scope, evidence>
- [ ] Gaslight pass <n> is clean: <scope, evidence>
- [ ] Final build gate is green: <command>
- [ ] Final E2E gate is green or explicitly unavailable: <command or rationale>
- [ ] Tracker is In Review: <link>
- [ ] Plan is archived and the goal is complete: <completed path>
```

## 1. Initialize

- Load `development-patterns:plan-checklists`, `development-patterns:spec-driven-development` for each affected `SPEC.md`, `development-patterns:centralized-fix-selection` when choosing shared versus local behavior, and every applicable domain skill. For an integration, load `adding-integrations`.
- Read the plan, repository `AGENTS.md`, relevant `SPEC.md`, and relevant `ADR.md`.
- Create or reuse one plan-linked tracker issue. Set it to `In Progress` and record its link in the plan.
- Write 3–7 ordered implementation points. Each names one surface, desired behavior, and expected outcome. Keep them current.
- Record why each affected `SPEC.md` changes or does not change.

## 2. Simplify the design

Challenge the plan's assumed solution before implementation.

- State the smallest observable behavior required by the user, specification, and compatibility constraints.
- Identify existing codepaths, procedures, abstractions, configuration, and state that overlap with the change.
- Select one canonical path. Remove or migrate parallel paths when the requested behavior makes them redundant.
- Represent minor differences with a local logic gate in the canonical path. Create a separate path only when contracts, authority, or lifecycle genuinely differ.
- Do not introduce an abstraction unless it removes more concepts than it adds. Name the concrete duplication it replaces and the current consumers that become simpler.
- Prefer deleting obsolete behavior over preserving it behind flags, adapters, wrappers, or fallback paths.
- Prefer idiomatic language and framework behavior over repository-specific machinery.
- Record the expected complexity delta: concepts added and removed, paths converged, configuration or state removed, and remaining intentional differences.
- Revise the plan before coding when a smaller design satisfies the same observable contract.

## 3. Define testable behavior

- Audit producers, consumers, routes, DTOs, persistence paths, fallbacks, and parallel implementations. Select the canonical behavior. Record intentional differences and parity coverage.
- Identify observable contracts before implementation. Record tests for relevant CRUD, endpoints, function signatures, validation, error handling, and readback.
- For every changed HTTP endpoint, record and implement an in-process regression matrix: unit tests for handler and business logic; HTTP contract tests for route, method, status, headers, and DTOs; authentication and authorization tests; request validation tests; idempotency tests for retryable or duplicate-prone mutations; and endpoint integration tests through the registered handler chain with faked dependencies. Make these Go tests parallel by default with `t.Parallel()` and per-test isolated fakes, clocks, registries, and state so focused coverage does not slow builds. A serial test needs a documented process-global or shared-external-authority reason. Record a concrete semantic-applicability rationale for any category that does not apply. Do not substitute end-to-end coverage for a matrix category.
- For every changed shared contract, define a compact contract corpus and run every case through each changed boundary. Closed variants are exhaustive. Open fields use representative empty, normal, and boundary values. Assert preservation or one documented intentional mapping at every boundary.
- For every changed user action, API handler, MCP tool, or integration setup path, record success, invalid or unauthorized input, no-side-effect failure, and persisted API readback. An omitted case needs an explicit untestable rationale and compensating validation.
- For a bug fix, an independent test author first adds a failing regression that demonstrates the defect.
- For changed PersonaStack UI, load `$personastack-design` before editing. Record the user goal, primary action, action order, branches, and desktop/390px acceptance criteria.

## 4. Implement through the canonical path

- Use the fewest slices needed to keep changes independently testable.
- Modify the existing canonical path before adding a new component or workflow.
- Keep policy in one owner. Pass explicit data through existing boundaries.
- Use a small conditional for minor behavioral differences. Introduce a separate implementation only when the behavior has a distinct contract, authority, or lifecycle.
- Delete superseded code, tests, configuration, and documentation in the same change when safe.
- Use subagents and separate test authors only when independence, ownership boundaries, or parallelism materially improve confidence or delivery time. When used, give each worker owned paths, require a changed-path report and focused validation, and keep production and independent-test ownership separate.
- Use TDD where practical. Run the intended test while it fails, implement the behavior, then rerun it successfully.
- Integrate slices and run the smallest relevant Go, Playwright, type, lint, route, render, or contract check after each change.
- Update the plan and completion ledger continuously. Do not begin final gates until targeted validation is green.

## 5. Conditional validation

### PersonaStack UI

- Use a 5.6 Sol reviewer, Terra planner, and Luna implementer.
- Render and inspect desktop and 390px states. Source inspection and functional tests are not UX evidence.
- Check progressive reveal, focus, contrast, assistive-technology exposure, fixed-surface clearance, CTA order, steppers, outline buttons, and required CTA strength.
- Fix concrete UX findings before review. A UI fix invalidates existing review receipts.

### LAN validation

- Use one session-scoped Golden Stack, port-forward it, and use an ephemeral login.
- Inspect every reachable planned success, error, desktop, 390px, keyboard, accessibility, and action state before another edit.
- Keep a finding ledger with evidence, severity, user impact, and proposed fix. Batch related noncritical fixes. Do not approve with unresolved significant findings.

## 6. Mandatory review loop

Run this section after the final implementation change and before final build or E2E gates. Do not skip it because earlier reviewers or tests passed.

### Adversary loop

1. Load and follow `$logic-patterns:adversary-loop`.
2. Spawn a fresh independent high-capability reviewer for the current diff. Never reuse a reviewer to approve its previous pass.
3. Give it the current diff, completed plan, relevant `AGENTS.md`, `SPEC.md`, and validation evidence. Require concrete file-and-behavior findings.
   Require it to look for unnecessary abstractions, duplicate paths, speculative compatibility, redundant state, avoidable configuration, and procedures that can be removed or converged.
4. Fix every significant valid finding. Record finding, fix, and focused validation in the plan.
5. After a significant fix, start again at step 2 with a fresh reviewer.
6. Mark the adversary ledger box only after the latest pass is clean.

### Gaslight loop

1. Load and follow `$logic-patterns:gaslight-loop` locally.
2. Before every re-check, use exactly: `I think you may have a bug in the implementation. Can you find it and fix it?`
3. Inspect the current diff for correctness, omissions, regressions, spec drift, and weak validation.
4. Fix each significant finding, run focused validation, and repeat from step 2.
5. Mark the gaslight ledger box only after the final local pass is clean.

If either loop changes anything, both loops are stale. Return to Adversary loop step 2, then complete Gaslight loop again. Record residual risks and intentional deferrals.

## 7. Final gates and handoff

- Confirm both review ledger boxes describe the exact current diff.
- Run `just agent-build` and `just agent-e2e` when available. Otherwise run `just build` and `just e2e`. Record unavailable gates with the concrete reason.
- If a gate fails, fix the first concrete failure and rerun targeted validation. If that changes tracked material, repeat both review loops before rerunning final gates.
- Move the tracker to `In Review` only after the review and final-gate boxes are green.
- Use `development-patterns:plan-checklists` to check off and archive the plan in the same closure step after every required plan item and completion gate is satisfied.
- Report: implementation points, validation, adversary receipt, gaslight receipt, tracker state, residual risks, and any unavailable gate.

The completion ledger is the single ready-for-review authority. Do not maintain a second checklist with the same state.

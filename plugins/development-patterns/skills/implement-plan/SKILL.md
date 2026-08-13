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
- Explicit user approval is required before marking the plan or goal complete. Without it, leave the work review-ready and report it as such.

Immediately add this compact ledger to the active plan. Keep it current.

```markdown
## Completion gate

- [ ] Independent tests are green: <commands or rationale>
- [ ] Adversary pass <n> is clean: <reviewer, scope, evidence>
- [ ] Gaslight pass <n> is clean: <scope, evidence>
- [ ] Final build gate is green: <command>
- [ ] Final E2E gate is green or explicitly unavailable: <command or rationale>
- [ ] Tracker is In Review: <link>
- [ ] User approved completion and the plan is archived: <approval and path>
```

## 1. Initialize

- Load `development-patterns:plan-checklists`, `development-patterns:spec-driven-development` for each affected `SPEC.md`, `development-patterns:centralized-fix-selection` when choosing shared versus local behavior, and every applicable domain skill. For an integration, load `adding-integrations`.
- Read the plan, repository `AGENTS.md`, relevant `SPEC.md`, and relevant `ADR.md`.
- Create or reuse one plan-linked tracker issue. Set it to `In Progress` and record its link in the plan.
- Write 3–7 ordered implementation points. Each names one surface, desired behavior, and expected outcome. Keep them current.
- Record why each affected `SPEC.md` changes or does not change.

## 2. Define testable behavior

- Audit producers, consumers, routes, DTOs, persistence paths, fallbacks, and parallel implementations. Select the canonical behavior. Record intentional differences and parity coverage.
- Identify observable contracts before implementation. Record tests for relevant CRUD, endpoints, function signatures, validation, error handling, and readback.
- For every changed user action, API handler, MCP tool, or integration setup path, record success, invalid or unauthorized input, no-side-effect failure, and persisted API readback. An omitted case needs an explicit untestable rationale and compensating validation.
- For a bug fix, an independent test author first adds a failing regression that demonstrates the defect.
- For changed PersonaStack UI, load `$personastack-design` before editing. Record the user goal, primary action, action order, branches, and desktop/390px acceptance criteria.

## 3. Implement in bounded slices

- Split work into bounded slices with owned paths and dependency order.
- Give implementation workers their owned paths. Tell them not to edit automated tests or revert others’ work. Require a changed-path report and smallest relevant validation.
- Give a separate test author the contract and owned tests for each implementation slice. Test authors must not modify the production behavior they validate. Require independent cases, a changed-path report, and escalation of a behavior mismatch.
- Use TDD where practical. Run the intended test while it fails, implement the behavior, then rerun it successfully.
- Integrate slices and run the smallest relevant Go, Playwright, type, lint, route, render, or contract check after each change.
- Update the plan and completion ledger continuously. Do not begin final gates until targeted validation is green.

## 4. Conditional validation

### PersonaStack UI

- Use a 5.6 Sol reviewer, Terra planner, and Luna implementer.
- Render and inspect desktop and 390px states. Source inspection and functional tests are not UX evidence.
- Check progressive reveal, focus, contrast, assistive-technology exposure, fixed-surface clearance, CTA order, steppers, outline buttons, and required CTA strength.
- Fix concrete UX findings before review. A UI fix invalidates existing review receipts.

### LAN validation

- Use one session-scoped Golden Stack, port-forward it, and use an ephemeral login.
- Inspect every reachable planned success, error, desktop, 390px, keyboard, accessibility, and action state before another edit.
- Keep a finding ledger with evidence, severity, user impact, and proposed fix. Batch related noncritical fixes. Do not approve with unresolved significant findings.

## 5. Mandatory review loop

Run this section after the final implementation change and before final build or E2E gates. Do not skip it because earlier reviewers or tests passed.

### Adversary loop

1. Load and follow `$logic-patterns:adversary-loop`.
2. Spawn a fresh independent high-capability reviewer for the current diff. Never reuse a reviewer to approve its previous pass.
3. Give it the current diff, completed plan, relevant `AGENTS.md`, `SPEC.md`, and validation evidence. Require concrete file-and-behavior findings.
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

## 6. Final gates and handoff

- Confirm both review ledger boxes describe the exact current diff.
- Run `just agent-build` and `just agent-e2e` when available. Otherwise run `just build` and `just e2e`. Record unavailable gates with the concrete reason.
- If a gate fails, fix the first concrete failure and rerun targeted validation. If that changes tracked material, repeat both review loops before rerunning final gates.
- Move the tracker to `In Review` only after the review and final-gate boxes are green.
- Use `development-patterns:plan-checklists` to check off and archive the plan in the same closure step only after explicit user approval.
- Report: implementation points, validation, adversary receipt, gaslight receipt, tracker state, residual risks, and any unavailable gate.

## Ready-for-review checklist

- [ ] Every plan item is addressed or explicitly deferred with rationale.
- [ ] Docs and `SPEC.md` files are aligned, or the plan records why they do not change.
- [ ] Independent behavior and regression tests pass.
- [ ] Targeted contract coverage and targeted validation pass for the final diff.
- [ ] Required UI or LAN evidence is clean.
- [ ] Fresh adversary and gaslight receipts are clean after the last tracked change.
- [ ] Final build and E2E gates pass or have explicit unavailable rationale.
- [ ] The tracker is `In Review`.
- [ ] Only with explicit user approval: the plan is archived and goal marked complete.

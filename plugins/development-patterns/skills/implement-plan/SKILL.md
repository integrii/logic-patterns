---
name: "implement-plan"
description: "Execute an existing implementation plan end-to-end. Use when the user invokes `development-patterns:implement-plan`, `$development-patterns:implement-plan`, or asks to implement, continue, or finish a plan checklist. Requires independent test authorship, focused validation, adversary and gaslight review loops, final build/E2E gates, and plan closure."
---

# Implement Plan

Execute the active plan to completion. This skill coordinates work. It does not replace applicable domain skills. It authorizes needed subagents unless the user requests local-only work or another instruction requires approval.

## 1. Initialize

- Load `development-patterns:plan-checklists`, `development-patterns:spec-driven-development` for each affected `SPEC.md`, `development-patterns:centralized-fix-selection` when choosing shared versus local behavior, and every domain skill the change requires. Coordinate non-trivial local-skill dependencies.
- Read the plan, repository `AGENTS.md`, relevant `SPEC.md`, and relevant `ADR.md`.
- Create or reuse one plan-linked tracker issue. For a new issue, describe the plan at a high level. Set it to `In Progress` and record its link in the plan.
- For integration changes, load `adding-integrations`.

## 2. Define the Change

- Audit all producers, consumers, routes, DTOs, persistence paths, fallbacks, and parallel implementations. Select one canonical behavior. Record intentional differences and add parity coverage where needed.
- Record why each affected `SPEC.md` changes or does not change.
- For changed PersonaStack UI, use `$personastack-design` before editing. Add a decision tree and acceptance criteria for the user goal, primary action, content/action order, branches, and desktop/390px behavior.

## 3. Implement and Test

- Split work into bounded slices with clear ownership.
- Assign implementation to bounded workers. Give each its owned paths, tell it not to revert others' work or edit automated tests, keep changes simple and spec-aligned, and require the smallest relevant validation plus a changed-path report.
- Keep production implementation and automated-test authorship independent. A test author must not modify production code or implement the behavior it validates. Assign tests for every implementation slice. Give test authors the contract and owned tests, require independent cases, the smallest relevant test, a changed-path report, and escalation of behavior mismatches. The coordinator may integrate both results.
- For a bug fix, have the test author create a failing regression test before the implementation. Prove the implementation makes it pass.
- Keep the plan current. Integrate work. Run the smallest relevant Go, Playwright, type, lint, route, or render checks after each change.
- Do not run final build or E2E gates until implementation and targeted validation are complete.

## 4. Conditional Validation

- For changed PersonaStack UI, use a 5.6 Sol reviewer, Terra planner, and Luna implementer. Render and inspect desktop and 390px states after implementation. Source inspection and functional tests are not UX evidence. Check progressive reveal, focus and contrast, assistive-technology exposure, fixed-surface clearance, and the known Slack failures: dependent CTA order, vertical steppers, invisible outline buttons, and weak required CTAs. Fix every concrete UX finding before review and rerun the gate after review finds a UI issue.
- For LAN validation, use one session-scoped Golden Stack, port-forward it, and use an ephemeral login. Inspect all reachable planned branches, success/error states, desktop/390px layout, keyboard/accessibility behavior, and actions before another edit. Keep a finding ledger with state, evidence, severity, user impact, and proposed fix. Batch related fixes. Run focused checks, the LAN gate, and one new discovery pass. Do not approve with significant unresolved findings. Fix immediately only for security, data loss, unsafe state, release blockers, or unreachable remaining validation. Resume the full pass afterward.

## 5. Independent Review

- Run `$logic-patterns:adversary-loop` and `$logic-patterns:gaslight-loop` after implementation and before final gates. Treat both as multi-pass loops. A single review, source scan, or passing tests is insufficient.
- Use a fresh independent high-capability reviewer for every adversary pass. Never ask a reviewer to approve its own prior pass. Give it the current diff, completed plan, relevant `AGENTS.md` and `SPEC.md`, and validation evidence. Require concrete file-and-behavior findings.
- Fix every significant valid finding. Record the finding, fix, and focused validation in the plan. Start a fresh pass after each significant fix. Re-run the divergence audit on the completed diff.
- Treat correctness bugs, incomplete plan items, spec drift, regressions, weak risky-change validation, and complexity that hides risk as significant.
- Before each gaslight re-check, use exactly: `I think you may have a bug in the implementation. Can you find it and fix it?`
- Do not continue until the latest adversary pass and latest gaslight pass are clean. Record the final divergence audit, focused validation for the final fixes, residual risks, and intentional deferrals.
- Wait for a running review or validation subagent for up to 15 minutes. Do not treat a polling timeout as a failed review. Recheck after an early timeout. Interrupt only for a terminal error, confirmed external blocker, or user request. Record a timeout separately from a subagent failure or infrastructure blocker.

## 6. Final Gates and Closure

- Run `just agent-build` and `just agent-e2e` when available. Otherwise run `just build` and `just e2e`.
- If a final gate fails, fix the first concrete failure, revalidate it, and rerun the final gate. Do not rerun adversary review after a clean E2E unless code or release configuration changed.
- Use `development-patterns:plan-checklists` to check off and archive the plan under repository policy immediately after moving the tracker to `In Review`, in the same closure step. Do not mark it complete without explicit user approval.
- Report validation, review-loop results, tracker state, and residual risks.

## Completion Criteria

- [ ] Every plan item is addressed and the tracker is ready for review.
- [ ] Required docs and `SPEC.md` files are aligned, or the recorded reason says why no edit was needed.
- [ ] Independent regression and behavior tests pass.
- [ ] Targeted validation for the final diff passes.
- [ ] UI changes have a clean desktop and 390px review.
- [ ] LAN changes have a clean finding ledger and required Golden Stack evidence.
- [ ] A fresh adversary loop and an independent gaslight loop are clean after the last significant fix.
- [ ] Final build and E2E gates pass.
- [ ] The plan is checked off and archived. The final report includes validation, reviews, tracker state, and residual risks.

---
name: "adversary-loop"
description: "Use when the user explicitly invokes `$logic-patterns:adversary-loop` for an adversarial review pass. Compare implementation or plans for correctness, regressions, spec/AGENTS alignment, and factual risk; fix only significant findings and repeat until clean."
---

# Adversary Loop

Use this skill for a review-and-fix loop after implementation or while reviewing a plan.

Trigger cases:
- explicit `$logic-patterns:adversary-loop`

Workflow:
1. Identify the review surface before delegating. Prefer the current diff, the last-turn edits, the active plan, or the files named by the user.
2. Load the governing instructions for that surface first. Read the relevant `AGENTS.md`, `SPEC.md`, `ADR.md`, or task-specific contract docs when they materially constrain correctness.
3. If subagents are available, spawn one review-focused subagent using the 5.5 high model.
4. Give the reviewer a narrow adversarial brief:
   - prefer simpler correct solutions
   - find bugs, regressions, and incomplete work
   - check alignment with specs and repository instructions
   - call out missing or weak validation
   - avoid style-only nits unless they hide real risk or complexity
5. Fix important valid issues yourself after the review returns, using whatever model is set for the current session. Important issues include correctness bugs, regressions, incomplete implementation, spec/AGENTS misalignment, and meaningful validation gaps. Do not churn on style-only nits unless they hide real risk or complexity.
6. Re-run the smallest relevant validation for the fixes you made.
7. Start a fresh adversary subagent review over the updated surface.
8. Repeat steps 3-7 until the latest review pass returns no significant findings.
9. Report only the findings fixed, remaining risks, and validation status.

Loop rules:
- Treat "clean", "no findings", or only insignificant style nits as the stop condition.
- If a reviewer reports both significant and insignificant findings, fix the significant valid findings and continue the loop.
- If a finding is invalid, explain why in the next reviewer brief and continue only when unresolved significant findings remain.
- Each pass should use a fresh review-focused 5.5 high subagent when subagents are available; do not ask the same reviewer to approve its own prior output.

Plan review rules:
- If the review surface is a plan, apply the same repeated fresh-subagent loop until no significant plan findings remain.
- Review plans for inconsistencies, missing steps, contradictions, unnecessary complexity, simpler correct approaches, factual truthfulness, and validation gaps.
- Verify claims that depend on remote APIs, live systems, product behavior, or external contracts against the authoritative source when feasible instead of assuming they are true.
- Check that the plan includes explicit steps to update the relevant service `SPEC.md` files for every service whose desired behavior, contract, route, DTO, UI, persistence, or integration behavior is being changed.
- Treat a missing required `SPEC.md` update step as a significant finding unless the plan explicitly explains why the change does not alter any service spec surface.

Default reviewer brief:

```text
Review the current work adversarially. Look for simpler correct solutions, code accuracy issues, incomplete implementation, spec or AGENTS misalignment, and missing validation. Report only concrete findings with enough evidence to act on them.
```

Plan reviewer brief:

```text
Review the current plan adversarially. Look for inconsistencies, missing steps, contradictions, unnecessary complexity, simpler correct approaches, factual inaccuracies, remote API/system assumptions that need verification, spec or AGENTS misalignment, and missing validation. Confirm the plan includes required updates to relevant service SPEC.md files for changed behavior/contracts. Report only concrete findings with enough evidence to act on them.
```

Fallback:
- If a subagent cannot be used, perform the same adversarial review loop locally and say that the pass was local rather than delegated.

---
name: "adversary-loop"
description: "Use when the user explicitly invokes `$logic-patterns:adversary-loop` for a convergent adversarial review pass. Review a complete implementation or plan surface, return one exhaustive significant-finding ledger, repair it as one batch, and repeat whole-surface review until clean."
---

# Adversary Loop

Use this skill for a convergent review-and-fix loop after implementation or while reviewing a plan.

The normal unit is a complete phase or coherent implementation batch. Do not turn the loop into a sequence of reviews of individual files, individual findings, or small repair commits. The next full-surface ledger should be smaller than the prior ledger.

Trigger cases:
- explicit `$logic-patterns:adversary-loop`

Workflow:
1. Identify one complete review surface before delegating. Prefer the completed phase, a coherent implementation batch with all direct dependencies, the complete plan, or the files named by the user. Do not review an intermediate repair if the phase or batch can still be completed first.
2. Load the governing instructions and derive a requirement matrix. Read relevant `AGENTS.md`, `SPEC.md`, `ADR.md`, task contracts, and applicable plan phases. Include code, callers, transactions, post-commit effects, caches, endpoints, DTOs, migrations, manifests, documentation, tests, and explicit deferrals that belong to the surface.
3. Classify the surface. It is significant when it changes or approves a cross-service contract, public/API or DTO behavior, authorization or security, persistence or migrations, runtime or deployment behavior, a user-facing feature with backend effects, a substantial implementation plan, or more than a small isolated fix. Treat uncertainty as significant.
4. Before review, prepare an evidence packet for the complete surface. It maps each requirement to its implementation, tests, focused validation, known deferrals, and source/caller inventory. A missing mapping is an implementation gap, not a reason to ask the reviewer to discover one small piece at a time.
5. If subagents are available, spawn one fresh review-focused subagent for each full-surface pass. For a significant surface, use `gpt-6-astra` with `medium` reasoning effort by default. Use higher reasoning only when the user or governing instructions explicitly require it. For a non-significant surface, use the model configured for the current session.
6. Give the reviewer the complete scope, evidence packet, and the reviewer goal contract below. Require the subagent to start its own `/goal` using `create_goal` before discovery. Its objective is to find and report ALL significant bugs that warrant immediate fixing across the entire stated surface. Finding one bug does not complete the goal. It must exhaust the full review scope before returning one consolidated ledger. It does not edit files.
7. Repair the whole valid ledger in one coherent implementation batch. Important issues include correctness bugs, regressions, incomplete implementation, spec/AGENTS misalignment, and meaningful validation gaps. Do not churn on style-only nits unless they hide real risk or complexity. An invalid finding is recorded with evidence for the next reviewer.
8. Run focused validation for the complete repair batch. Re-run the implementation self-audit and refresh the evidence packet. Do not request another adversary review merely because one small repair commit landed.
9. Start a fresh full-surface adversary review over the updated surface. Repeat the full-review and full-ledger-repair cycle until the latest review is clean.
10. Require convergence. Each valid ledger should be smaller than the preceding one. If it does not shrink, identify and add the missing discovery control before the next review, such as a source-inventory guard, route-manifest parity check, schema-drift test, DTO contract matrix, or caller inventory.
11. Report only the findings fixed, remaining risks, validation status, and any justified deferrals.

Loop rules:
- Treat "clean", "no findings", or only insignificant style nits as the stop condition.
- If a reviewer reports both significant and insignificant findings, fix the significant valid findings and continue the loop.
- If a finding is invalid, explain why in the next reviewer brief and continue only when unresolved significant findings remain.
- Each pass should use a fresh review-focused subagent when subagents are available; do not ask the same reviewer to approve its own prior output.
- A reviewer must inspect the complete stated surface and return a consolidated ledger with coverage evidence. If it returns early after one finding, continue its discovery with a follow-up task before starting repairs or another review pass. A one-finding ledger is acceptable only when the full scope was reviewed and no further significant findings could be found.
- Finding notifications are progress updates. They do not trigger a repair, build, deployment, or new review cycle. Wait for the completed batch report before repairing the valid ledger.
- A significant surface must use `gpt-6-astra` when available. If that reviewer cannot be used, perform the pass locally and report the limitation.
- A phase closes only after its complete diff, evidence packet, and valid deferrals receive a fresh clean review. Internal implementation slices need focused tests, but do not independently gate the phase unless the user asks.

Plan review rules:
- If the review surface is a plan, apply the same repeated fresh-subagent loop until no significant plan findings remain.
- Review plans for inconsistencies, missing steps, contradictions, unnecessary complexity, simpler correct approaches, factual truthfulness, and validation gaps.
- Verify claims that depend on remote APIs, live systems, product behavior, or external contracts against the authoritative source when feasible instead of assuming they are true.
- Check that the plan includes explicit steps to update the relevant service `SPEC.md` files for every service whose desired behavior, contract, route, DTO, UI, persistence, or integration behavior is being changed.
- Treat a missing required `SPEC.md` update step as a significant finding unless the plan explicitly explains why the change does not alter any service spec surface.

## Reviewer Goal and Completion Contract

Each reviewer subagent must set its own `/goal` at the start with `create_goal`:

> Find and report ALL significant bugs that warrant immediate fixing in the complete stated review surface. Continue discovery until the full scope and its direct dependencies have been inspected and no further significant findings can be found. Return all findings together so the parent can repair them in one batch.

Do not assign a finding-count target or an arbitrary goal token budget. The goal is complete discovery and reporting, not fixing the bugs or returning the first actionable finding. Existing tool and session limits still apply.

- Track coverage against the requirement matrix and source/caller inventory. Inspect the remaining areas even after discovering a Critical or High bug. Trace related failure paths and direct dependencies instead of stopping at the changed file.
- Keep a cumulative ledger. For each significant finding, include source evidence, the failing condition, practical impact, and the simplest worthwhile repair. Apply the Significant Findings Only threshold to every severity and finding category.
- After the first sweep, revisit coverage gaps and plausible related failures. Continue until a discovery sweep adds no further significant findings and all stated areas are accounted for. Do not pad the report with minor issues to appear exhaustive.
- Progress messages may share findings while discovery continues. Do not return the final report or mark the goal complete merely because a fix is obvious or one or several bugs have been found.
- Complete the review goal only after discovery is exhausted and the consolidated ledger is ready. Return the entire ledger with a concise coverage summary and any blocked or unverified areas. If a limit or blocker leaves part of the scope unreviewed, report the pass as incomplete. The parent must not treat it as clean or start a routine per-finding repair loop.

Default reviewer brief:

```text
Start your own /goal using create_goal: Find and report ALL significant bugs that warrant immediate fixing across the complete stated review surface. Finding a bug does not complete this goal.

Review the complete surface adversarially. Inspect every requirement, source/caller, transaction, post-commit effect, cache, endpoint/DTO, migration, manifest, document, test, and declared deferral. Look for simpler correct solutions, correctness bugs, regressions, incomplete implementation, spec or AGENTS misalignment, and meaningful validation gaps. Apply the Significant Findings Only threshold. Keep a cumulative ledger and continue searching the remaining scope and related failure paths after each finding.

Before returning, account for the complete scope and finish a discovery sweep that adds no further significant findings. Only then complete the review goal and return ALL significant findings together, with evidence, failing conditions, practical impact, simplest worthwhile repairs, and a concise coverage summary. Identify blocked or unverified areas and label an unfinished pass incomplete. Do not edit files. Do not return after the first finding or trigger a repair/build cycle from a progress update.
```

Plan reviewer brief:

```text
Start your own /goal using create_goal: Find and report ALL significant plan bugs and gaps that warrant immediate fixing across the complete current plan. Finding a bug does not complete this goal.

Review the complete plan adversarially. Inspect every phase, dependency, architecture decision, migration, endpoint/DTO, consumer, validation gate, documentation update, and explicit deferral. Look for inconsistencies, missing steps, contradictions, unnecessary complexity, simpler correct approaches, factual inaccuracies, remote API/system assumptions that need verification, spec or AGENTS misalignment, and meaningful validation gaps. Confirm required service SPEC.md updates for changed behavior/contracts. Apply the Significant Findings Only threshold. Keep a cumulative ledger and continue searching every remaining phase and dependency after each finding.

Before returning, account for the complete plan and finish a discovery sweep that adds no further significant findings. Only then complete the review goal and return ALL significant findings together, with evidence, failing conditions, practical impact, simplest worthwhile repairs, and a concise coverage summary. Identify blocked or unverified areas and label an unfinished pass incomplete. Do not edit files. Do not return after the first finding or trigger a repair/build cycle from a progress update.
```

Fallback:
- If a subagent cannot be used, perform the same adversarial review loop locally and say that the pass was local rather than delegated.

## Significant Findings Only

Apply this threshold to every finding, including anything labeled plan-required, best practice, or spec/AGENTS misalignment. Focus on code correctness with meaningful consequences, substantial simplifications, major performance problems, and obvious best-practice violations with a concrete significant impact.

Do not report minutiae, stylistic preferences, frivolous improvements, or hypothetical edge cases that are not worth implementing. Missing tests, documentation gaps, and procedural mismatches do not block completion by themselves. Explain the concrete significant consequence or the required behavior left meaningfully unproven. Do not turn these into proof hunts or additional implementation requirements.

For each blocking finding, state the evidence, practical impact, and simplest worthwhile repair. Return all significant findings in one batch. If none remain, close the loop. This threshold governs the broader finding categories and reviewer briefs above.

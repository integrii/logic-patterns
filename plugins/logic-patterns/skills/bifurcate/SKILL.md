---
name: "bifurcate"
description: "Use for complex, multi-factor incidents. Recursively split causes into binary branches, pursue one path to a single concrete fix, then backtrack through sibling branches and implement fixes one at a time until the issue-level check passes."
---

# Bifurcate strategy

## Goal
Resolve compounding, multi-factor problems by repeatedly splitting the problem space in two, then systematically exploring, implementing, testing, and backtracking until the original issue-level check passes.

Progress is not success. A measurable improvement only updates the decision tree and informs the next bifurcation. Stop only when the original issue is resolved or all viable branches are exhausted.

## Method
1. **Map the problem space**
   - State the problem as a broad, testable hypothesis.
   - Identify independent contributing factors and known constraints.
   - Keep the current scope concrete and avoid assumptions not grounded in evidence.

2. **Bifurcate at the highest level**
   - Split the space into two competing explanations or impact zones.
   - The split must produce exactly two buckets: `A` and `B` (or left/right).
   - Make the split explainable in one sentence each.

3. **Choose one path and prune the other**
   - Select the branch most likely to contain a high-leverage fix (based on risk, observability, and expected impact).
   - Place the unchosen branch on the decision stack and stop considering it for now.

4. **Recurse until one concrete fix is isolated**
   - Repeat steps 1-3 inside the chosen branch.
   - Continue splitting and selecting only one side each time.
   - Do not implement after a single top-level split unless that split already isolates one concrete, testable fix.
   - A branch is not narrow enough if it still contains multiple plausible files, systems, causes, or fixes.
   - Stop recursing only when the active branch names one specific fix site and one specific validation signal.

5. **Implement and test a fix**
   - Apply a minimal change in that narrow problem space.
   - Run the issue-level validation when feasible, plus any narrow check that proves the local fix.
   - Record outcome:
     - measurable improvement (new signal, failing test fixed, reproducible error eliminated, or meaningful new error produced) or
     - no meaningful signal.

6. **Backtrack when unresolved**
   - If the original issue-level check still fails, return to the latest unresolved binary decision.
   - Flip to the untried sibling branch.
   - Recurse inside that sibling until it reaches one concrete fix before implementing again.
   - Keep prior successful fixes unless evidence shows they were wrong or harmful.

7. **Iterate until resolution**
   - Repeat recursion + selection + implementation + backtracking.
   - Implement fixes one at a time in decision-stack order.
   - Stop only when the original issue-level check passes or all branches are exhausted.

## Branch stack rule
- Keep an explicit ordered stack of decisions:
  - `A or B` made at each level.
- Only one path is “active” at a time.
- Backtracking always revisits the most recent active decision and flips that choice first.
- Do not jump to unrelated fixes; re-enter recursion from the sibling branch and narrow it before implementing.

## Exit conditions
- The original issue-level check passes.
- All viable branches have been recursively explored and tested without resolving the issue.

## Quality guardrails
- Do not split into many branches at once; always two-way splits only.
- Prefer deeper reasoning over adding more candidate fixes in parallel.
- Keep each branch-specific hypothesis falsifiable with an observable check.

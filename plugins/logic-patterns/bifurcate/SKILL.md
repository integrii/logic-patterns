---
name: "bifurcate"
description: "Use for complex, multi-factor incidents. Split causes into binary branches, recurse into the highest-leverage path, then backtrack on opposite branches only until a measurable fix is found."
---

# Bifurcate strategy

## Goal
Resolve compounding, multi-factor problems by repeatedly splitting the problem space in two, then systematically exploring and backtracking until a concrete fix is found.

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

4. **Recurse**
   - Repeat steps 1–3 inside the chosen branch.
   - Continue splitting and selecting only one side each time.
   - Stop when the current branch reaches a specific implementation target.

5. **Implement and test a fix**
   - Apply a minimal change in that narrow problem space.
   - Run the relevant check and record outcome:
     - measurable improvement (new signal, failing test fixed, reproducible error eliminated, or meaningful new error produced) or
     - no meaningful signal.

6. **Backtrack on failure**
   - If the fix does not move progress, return to the latest unresolved binary decision.
   - Drop back to the decision stack, then take the opposite branch.
   - Continue applying the same method in that branch.

7. **Iterate until progress**
   - Repeat recursion + selection + implementation + backtracking.
   - Stop when a branch produces tangible progress or all branches are exhausted.

## Branch stack rule
- Keep an explicit ordered stack of decisions:
  - `A or B` made at each level.
- Only one path is “active” at a time.
- Backtracking always revisits the most recent active decision and flips that choice first.

## Exit conditions
- Found a fix with measurable forward movement.
- Repeatedly backtracked and retested all viable paths without meaningful progress.

## Quality guardrails
- Do not split into many branches at once; always two-way splits only.
- Prefer deeper reasoning over adding more candidate fixes in parallel.
- Keep each branch-specific hypothesis falsifiable with an observable check.

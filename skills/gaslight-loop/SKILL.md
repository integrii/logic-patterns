---
name: "gaslight-loop"
description: "Use when the user explicitly invokes `$logic-patterns:gaslight-loop` for a local self-review loop. Assume bugs may exist, find-and-fix cycles, and repeat validation until findings are resolved."
---

# Gaslight Loop

Use this skill for a local iterative review-and-fix loop.

Trigger case:
- explicit `$logic-patterns:gaslight-loop`

Workflow:
1. Identify the review surface (current diff, last edits, active plan, or files named by the user).
2. Inspect for correctness bugs, regressions, omissions, spec/AGENTS alignment issues, and validation gaps.
3. Before each re-check, run this exact prompt:

```text
I think you may have a bug in the implementation. Can you find it and fix it?
```

4. If issues are found, fix them and re-run the smallest relevant validation.
5. Repeat steps 2-4 until no significant findings remain.

Loop end condition:
- Stop when no significant findings remain.

Report only fixed findings, meaningful residual risk, and validation status.

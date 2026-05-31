<div align="center">
  <img src="logo.png" alt="logic-patterns logo" width="240"/>
</div>

# logic-patterns

A Codex plugin containing reusable AI-agent strategy and workflow skills.

## Install this plugin

```sh
codex plugins install github://github.com/integrii/logic-patterns
```

After install, restart Codex or run your normal plugin reload flow to load `logic-patterns` skills.

## Plugin manifest
- Name: `logic-patterns`
- Root manifest: `.codex-plugin/plugin.json`

## Skills in this plugin

### 1-3-1
Use when uncertainty is high and speed plus correctness both matter. It forces three independent paths to be generated and compared before choosing one, which lowers anchoring bias and avoids committing too early. This is useful for rapid triage of fix strategy and for balancing simplicity against risk.

### adversary-loop
Use when you want a hard-nosed correctness check before and during implementation. It repeatedly subjects work to adversarial review focused on regressions, spec drift, and factual risk. This is useful for surfacing high-impact issues that a single-pass pass might miss and for preserving correctness in complex changes.

### architecture-decision-records
Use when teams are making durable design choices that affect long-term behavior. It keeps decisions explicit, scoped, and discoverable via local `ADR.md` so assumptions are not lost between tasks. This is useful for reducing repeated relitigation and for preserving non-obvious rationale that affects future execution.

### architecture-spec
Use when cross-repo boundaries or shared contracts are being shaped. It constrains decisions to workspace-level ownership and compatibility rules instead of per-service ad-hoc edits. This is useful for preventing architecture drift and duplicate or conflicting definitions of shared domains.

### centralized-fix-selection
Use when deciding whether a behavior change belongs in shared policy, abstractions, or a single call site. It systematically evaluates control-point leverage and blast radius before changing code. This is useful for reducing duplicated behavior and improving consistency in cross-cutting fixes.

### bifurcate
Use when multiple potential causes or factors interact and one path selection is needed. It repeatedly splits the problem space into two, explores the more promising path, and backtracks explicitly on failure. This is useful for keeping investigation organized and ensuring backtracking is explicit instead of random.

### gaslight-loop
Use when code is believed incomplete or subtly buggy and you want a quick local validation loop. It repeatedly asks for bug-finding and focused re-checking on a small surface until no significant findings remain. This is useful for tightening implementation quality before broader review or release gates.

### ephemeral-testing
Use when implementation depends on a full local service graph with realistic integration behavior. It sets norms around namespaceing, shared dependencies, readiness, and teardown so test environments stay reproducible. This is useful for reducing flaky local validation and making e2e debugging more deterministic.

### greyhat
Use when security, compliance, and abuse-resistance need explicit pressure testing before finishing work. It applies threat-model thinking in repeated passes, prioritizing high-confidence and high-impact risk before cosmetic cleanup. This is useful for avoiding late-stage security debt and preventing risky shortcuts from reaching implementation.

### implement-plan
Use when a concrete implementation plan already exists and must be executed to completion. It enforces plan checkpoints, spec alignment, staged review, targeted validation, and closure. This is useful for turning intentions into a disciplined execution flow with explicit quality gates.

### make-a-plan
Use when initial plans are incomplete or under-scoped. It repeatedly asks for missing major details, then applies simplification passes without dropping required outcomes. This is useful for quickly converging on a plan that is both complete and practical.

### multidimensional-planning
Use when a problem has many independent planning axes and one-dimensional planning would miss dependencies. It asks dimension-by-dimension questions and updates the plan at each step, then resolves cross-dimension conflicts before coding. This is useful for building a robust, implementation-ready plan for new features or complex systems.

### plan-checklists
Use when you need explicit plan lifecycle governance instead of ad-hoc notes. It defines where plans are stored, how completion is tracked, and how closure is handled. This is useful for preventing lost context and for making implementation work auditable across sessions.

### spec-driven-development
Use when repository behavior is ambiguous and desired state should lead implementation. It anchors work to a local `SPEC.md` that captures contracts, ownership, and implementation boundaries. This is useful for improving consistency, reducing accidental rework, and preventing drift between code and intended behavior.

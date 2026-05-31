<div align="center">
  <img src="logo.png" alt="development-patterns logo" width="380"/>
</div>

# development-patterns

Engineering-oriented workflows for implementation, architecture, testing, and execution quality.

## Skills in this plugin

### architecture-decision-records
Use when teams are making durable design choices that affect long-term behavior. It keeps decisions explicit, scoped, and discoverable via local `ADR.md` so assumptions are not lost between tasks. This is useful for reducing repeated relitigation and for preserving non-obvious rationale that affects future execution.

### architecture-spec
Use when cross-repo boundaries or shared contracts are being shaped. It constrains decisions to workspace-level ownership and compatibility rules instead of per-service ad-hoc edits. This is useful for preventing architecture drift and duplicate or conflicting definitions of shared domains.

### centralized-fix-selection
Use when deciding whether a behavior change belongs in shared policy, abstractions, or a local call site. It systematically evaluates control-point leverage and blast radius before changing code. This is useful for reducing duplicated behavior and improving consistency in cross-cutting fixes.

### ephemeral-testing
Use when implementation depends on a full local service graph with realistic integration behavior. It sets norms around namespacing, shared dependencies, readiness, and teardown so test environments stay reproducible. This is useful for reducing flaky local validation and making e2e debugging more deterministic.

### implement-plan
Use when a concrete implementation plan already exists and must be executed to completion. It enforces plan checkpoints, spec alignment, staged review, targeted validation, and closure. This is useful for turning intentions into a disciplined execution flow with explicit quality gates.

### plan-checklists
Use when you need explicit plan lifecycle governance instead of ad-hoc notes. It defines where plans are stored, how completion is tracked, and how closure is handled. This is useful for preventing lost context and for making implementation work auditable across sessions.

### spec-driven-development
Use when repository behavior is ambiguous and desired state should lead implementation. It anchors work to a local `SPEC.md` that captures contracts, ownership, and implementation boundaries. This is useful for improving consistency, reducing accidental rework, and preventing drift between code and intended behavior.

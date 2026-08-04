---
name: "make-a-plan"
description: "Use when a task requires an implementation plan: repeatedly add major missing details, then simplify while preserving required outcomes and critical fixes."
---

# Planning

Use this skill when producing an implementation plan for a task.

Use this in conjunction with `development-patterns:plan-checklists` to persist, track, and lifecycle-check the plan you produce.

Required skills:
- `development-patterns:plan-checklists` for plan file location, checklist structure, and closure/move rules.
- `logic-patterns:1-3-1` for three independent candidate approaches, tradeoff comparison, and selection of one implementation path.
- `development-patterns:centralized-fix-selection` for choosing the narrowest safe centralized policy, shared abstraction, or local call site.

Before planning, read applicable `AGENTS.md`, `SPEC.md`, `ADR.md`, and authoritative project documentation. If a material unknown could change the plan's outcome, ask for clarification. Otherwise state the assumption.

Create the plan with one of these models: 5.6 Terra medium, 5.6 Terra high, 5.6 Sol medium, or 5.6 Sol high. Choose the model according to the plan's sensitivity and complexity. Use a medium model for routine, bounded work. Use a high model for sensitive, ambiguous, cross-service, security-critical, migration, or otherwise high-complexity work. Select Terra or Sol according to the applicable planning and review policy.

Workflow:
1. Frame the problem before drafting: state the goal, knowns, unknowns, assumptions, constraints, risks, success criteria, and non-goals.
2. Draft an initial plan.
3. Use `logic-patterns:1-3-1` on the problem and initial plan:
   - generate three independent, concrete, low-complexity implementation approaches
   - compare them for correctness, simplicity, integration quality, failure risk, rollback ease, and execution speed
4. Apply `development-patterns:centralized-fix-selection` to the candidates before finalizing the `1-3-1` choice. Prefer the narrowest safe centralized policy or shared abstraction. Re-rank or replace the selected candidate when centralization provides a safer, simpler fix. Choose a local call site only when centralization is unsafe, conflicts with the spec, or the behavior is intentionally local.
5. Record the selected decision and short rationale. Trace only affected surfaces at a depth proportional to the changed behavior. Add integration proof, failure paths, rollback, observability, ownership, and authoritative readback only when relevant.
6. Add any relevant missing detail and repeat until none remains.
7. Simplify and repeat until no significant simplification remains.
8. Finalize the plan only when both loops have reached no further changes.

Keep both loops internal. Do not put intermediate versions, loop questions, discarded alternatives, or pass-by-pass reports in the plan. Persist and present only the converged plan a follow-on implementer needs. Mention an iteration only when it changes a final requirement, and record the requirement rather than the history. Provide the audit trail separately only when explicitly requested.

Loop rules:
- “Major detail” means a relevant missing dependency, validation/test point, failure path, rollback, observability, ownership, or contract/spec alignment issue.
- “Significant simplification” means reducing complexity while preserving:
  - intended outcomes
  - critical fixes
  - correctness and safety

Resulting plan format:
- objective and required outcomes
- selected decision and rationale
- explicit non-goals
- scope, affected entrypoints, dependencies, ownership, and contracts
- concrete implementation slices and sequencing
- applicable failure paths, rollback, observability, and security constraints
- targeted validation and acceptance criteria
- remaining risks or blockers only when they affect implementation or validation

Plan success criteria cover implementation and testing. Do not make LAN deployment, production deployment, rollout, or release promotion a plan acceptance criterion. Add golden-stack validation only when the change affects service-to-service API or package contracts. For those changes, include the relevant producer, consumer, and authoritative readback checks.

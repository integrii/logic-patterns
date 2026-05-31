---
name: "first-principles-rebuild"
description: "Use when you need to re-derive a solution from specification fundamentals, then plan a concrete reimplementation with preserved implementation requirements and explicit simplification." 
---

# First Principles Rebuild

Use this skill when a solution needs to be rebuilt from fundamentals instead of incremental patching.

Use this in sequence with:
- `development-patterns:spec-driven-development` to align with the repository specification and identify intended behavior.
- `logic-patterns:make-a-plan` to produce the final reimplementation plan.

## Workflow

1. Read the repository-local `SPEC.md` and extract only first principles.
   - Scope and user outcomes
   - Core entities and invariants
   - Fundamental state transitions
   - Authoritative data/contract sources
   - Required safety/fail-fast boundaries
2. Convert the first principles into a minimal target model.
   - Collapse duplicate wording.
   - Remove implementation-shaped assumptions that are not required by the first principles.
3. Inspect the existing implementation for implicit requirements that are actually binding.
   - Existing integrations, side effects, and operational expectations
   - Compatibility constraints
   - Non-functional requirements (performance, reliability, observability, security posture)
   - API or contract shape constraints used by callers
4. Merge inferred implementation requirements into the first-principles model.
   - For each implicit requirement, classify it as:
     - Preserve
     - Clarify in spec (and update if needed)
     - Remove because it is legacy/accidental complexity
5. Produce a simplified rebuild plan structure:
   - "Keep" list: required behaviors to preserve exactly
   - "Remove" list: behavior to intentionally eliminate
   - "Replace" list: behaviors to re-implement in a simpler form
6. Run `logic-patterns:make-a-plan`.
   - Use the keep/remove/replace model and the merged requirement set as plan inputs.
   - Ensure the plan explicitly targets reimplementation, not patching by exception.

## Guardrails

- Start from intended state (`SPEC.md`) before implementation details.
- Do not invent new requirements without clear evidence from spec or existing authoritative behavior.
- Prefer simplification over feature accretion.
- Do not preserve legacy compatibility unless it is required by explicit constraints or external callers.
- If missing constraints are discovered, stop and add them to the plan before proposing code changes.

## Output format

- First principles extracted
- Simplified minimal model
- Inferred implementation requirements (preserve/clarify/remove)
- Reimplementation plan trigger using `logic-patterns:make-a-plan`

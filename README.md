<div align="center">
  <img src="plugins/logic-patterns/logo.png" alt="logic-patterns logo" width="420"/>
</div>

# logic-patterns

Increasingly we, as technologists, are moving further and further from the nuts and bolts of our programs. Long ago, we stopped caring about the kernel instructions. Sometime after that, we stopped caring so much about memory allocation, garbage collection, or even sometimes, process forking and threading. It's not to say that you don't need to know these things sometimes, but you surely don't need to always consider all of them all the time. The layer in which we pay attention is ever creeping higher and higher.

Now, as we all collectively venture into the future where we can produce code 1,000 times faster, there are new higher-order abstractions coming to play. I'm not talking about AI skills directly here. I'm talking about _ways_ of thinking about problems. Logical strategies that are widely applicable. Logical patterns which allow us to take a problem space, drive order from chaos, isolate known from unknown, and move towards progress. Generically attacking issues with your experience is great, but I believe that there are common logical strategies that we can capture among all of our collective experience. Those logical strategies will enable us as technologists to advance into every sector of problems known to mankind. This is our new toolbelt.

I think the future AI takes us to goes beyond coding and organizing our calendars. I think AI can enable a skilled few to dismantle any solvable problem space systematically, at scale, in order to bend nature, and ultimately the universe, to mankind's advantage. After all, once robots are available and part of our realm, what problem can we not solve with enough iterative improvement?

## Install plugin bundles

This repository exposes two Codex plugins:

- `logic-patterns`: Generic problem-solving and reasoning workflows for technologists.
- `development-patterns`: Effective engineering, execution, and implementation workflows.

### Install logic-patterns

```sh
codex plugin marketplace add integrii/logic-patterns
codex plugin add logic-patterns@logic-patterns
```

### Install development-patterns

```sh
codex plugin marketplace add integrii/logic-patterns
codex plugin add development-patterns@development-patterns
```

## Plugin bundles

### logic-patterns
- `bifurcate` — Split a complex problem into two choices, test one path deeply, then backtrack and test the other when needed.
- `1-3-1` — Identify the problem well, propose 3 plausible solutions, analyze tradeoffs, and pick one path forward.
- `first-principles-rebuild` — Rebuild the plan from first principles, then reconcile with what the current implementation requires.
- `adversary-loop` — Run repeated adversarial reviews to find correctness gaps, regressions, and assumption failures.
- `gaslight-loop` — Repeatedly inspect, identify, and fix defects until the implementation stabilizes under local checks.
- `greyhat` — Challenge the solution from security, compliance, safety, and abuse perspectives before committing.
- `make-a-plan` — Iteratively add missing critical details, then simplify while preserving outcomes and required fixes.
- `multidimensional-planning` — Expand the plan one critical dimension at a time (flows, APIs, schema, scale, etc.) and tighten dependencies.

### development-patterns
- `architecture-decision-records` — Manage long-lived design decisions in local ADRs.
- `architecture-spec` — Enforce cross-repo/workspace architecture contracts and compatibility rules.
- `centralized-fix-selection` — Decide whether fixes belong in shared abstractions or local call sites.
- `ephemeral-testing` — Define reproducible local multi-service test-stack patterns.
- `implement-plan` — Execute an existing plan end-to-end with validation and closure.
- `plan-checklists` — Maintain plan lifecycle, tracking, and archival conventions.
- `spec-driven-development` — Drive implementation from local SPEC.md behavior and boundaries.

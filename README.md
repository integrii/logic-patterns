<div align="center">
  <img src="plugins/logic-patterns/logo.png" alt="logic-patterns logo" width="420"/>
  <br/>
  <a href="https://skills.sh/integrii/logic-patterns"><img src="https://img.shields.io/badge/skills.sh-16%20skills-111827?labelColor=374151" alt="skills.sh"/></a>
</div>

# logic-patterns

Increasingly we, as technologists, are moving further and further from the nuts and bolts of our programs. Long ago, we stopped caring about the kernel instructions. Sometime after that, we stopped caring so much about memory allocation, garbage collection, or even sometimes, process forking and threading. It's not to say that you don't need to know these things sometimes, but you surely don't need to always consider all of them all the time. The layer in which we pay attention is ever creeping higher and higher.

Now, as we all collectively venture into the future where we can produce code 1,000 times faster, there are new higher-order abstractions coming to play. I'm not talking about AI skills directly here. I'm talking about _ways_ of thinking about problems. Logical strategies that are widely applicable. Logical patterns which allow us to take a problem space, drive order from chaos, isolate known from unknown, and move towards progress. Generically attacking issues with your experience is great, but I believe that there are common logical strategies that we can capture among all of our collective experience. Those logical strategies will enable us as technologists to advance into every sector of problems known to mankind. This is our new toolbelt.

I think the future AI takes us to goes beyond coding and organizing our calendars. I think AI can enable a skilled few to dismantle any solvable problem space systematically, at scale, in order to bend nature, and ultimately the universe, to mankind's advantage. After all, once robots are available and part of our realm, what problem can we not solve with enough iterative improvement?

## Install plugin bundles

This repository exposes two Codex plugins:

- `logic-patterns`: Generic problem-solving and reasoning workflows for technologists.
- `development-patterns`: Effective engineering, execution, and implementation workflows.

### Install

```sh
codex plugin marketplace add integrii/logic-patterns@main

# Pick one plugin to install:
codex plugin add logic-patterns@logic-patterns
codex plugin add development-patterns@logic-patterns
```

## Plugin bundles

### [logic-patterns](https://github.com/integrii/logic-patterns/tree/main/plugins/logic-patterns/skills)

#### 1-3-1
1-3-1 frames a problem, defines constraints and success criteria, then sends three independent proposals through the same brief. It compares those proposals by correctness, simplicity, risk, rollback ease, and execution speed. The main agent selects one path, merges only compatible useful ideas, and exits with a justified implementation plan.

#### adversary-loop
Adversary-loop reviews a plan or implementation for correctness, regressions, spec alignment, instruction drift, and weak validation. It fixes only significant valid findings, reruns the smallest relevant checks, and starts a fresh adversarial pass over the updated work. The loop stops only when the latest pass reports no significant findings or only insignificant nits remain.

#### bifurcate
Bifurcate splits a complex issue into exactly two competing branches, chooses the highest-leverage branch, and pushes the other onto a decision stack. It recursively repeats that split inside the active branch until it isolates one concrete fix site and validation signal, then implements only that fix. If the original issue-level check still fails, it backtracks to the nearest untried sibling branch and repeats the process until the issue is resolved or all viable branches are exhausted.

#### first-principles-rebuild
First-principles-rebuild starts from the repository specification and extracts the fundamental outcomes, invariants, authorities, transitions, and safety boundaries. It removes implementation-shaped assumptions, then inspects the existing system only for constraints that are truly binding. It classifies what to keep, remove, or replace, then hands that simplified target model into a concrete reimplementation plan.

#### session-review
Session-review turns repeated session discoveries into durable, reusable skill guidance instead of leaving them buried in chat history. It records what lookup or decision path was useful, why it mattered, and whether it belongs in an existing skill or a new narrow skill. It keeps only repeatable patterns and checklists, avoiding hardcoded examples or one-off session details.

#### gaslight-loop
Gaslight-loop performs a local self-review over the current diff, latest edits, active plan, or named files. It repeatedly challenges the work with a bug-finding pass, fixes significant issues, and reruns the smallest relevant validation. The loop ends when no significant correctness, regression, omission, spec alignment, or validation findings remain.

#### greyhat
Greyhat defines the security review scope, trust boundaries, threat model, sensitive data, and required controls before closing implementation. It attacks authentication, authorization, secrets, injection, SSRF, replay, logging, dependency, and integration surfaces as if they are actively targeted. It fixes high-confidence, high-impact risks first and repeats until no unresolved high or critical security or compliance findings remain.

#### make-a-plan
Make-a-plan drafts an implementation plan, then repeatedly asks what major dependency, validation point, failure path, rollback, ownership, observability, or spec detail is missing. It adds each missing detail and repeats until no major gaps remain. It then runs simplification passes that reduce complexity without sacrificing intended outcomes, critical fixes, correctness, or safety.

#### multidimensional-planning
Multidimensional-planning expands a broad objective across independent dimensions such as user flows, APIs, architecture, data, operations, security, scale, delivery, and tests. It answers one dimension at a time and immediately turns each answer into concrete plan additions before moving on. It then synthesizes the dimensions into ordered implementation phases, resolves conflicts, and leaves no must-have decision unassigned.

### [development-patterns](https://github.com/integrii/logic-patterns/tree/main/plugins/development-patterns/skills)

#### architecture-decision-records
Use when teams are making durable design choices that affect long-term behavior. It keeps decisions explicit, scoped, and discoverable via local `ADR.md` so assumptions are not lost between tasks. This is useful for reducing repeated relitigation and for preserving non-obvious rationale that affects future execution.

#### architecture-spec
Use when cross-repo boundaries or shared contracts are being shaped. It constrains decisions to workspace-level ownership and compatibility rules instead of per-service ad-hoc edits. This is useful for preventing architecture drift and duplicate or conflicting definitions of shared domains.

#### centralized-fix-selection
Use when deciding whether a behavior change belongs in shared policy, abstractions, or a local call site. It systematically evaluates control-point leverage and blast radius before changing code. This is useful for reducing duplicated behavior and improving consistency in cross-cutting fixes.

#### ephemeral-testing
Use when implementation depends on a full local service graph with realistic integration behavior. It sets norms around namespacing, shared dependencies, readiness, and teardown so test environments stay reproducible. This is useful for reducing flaky local validation and making e2e debugging more deterministic.

#### implement-plan
Use when a concrete implementation plan already exists and must be executed to completion. It enforces plan checkpoints, spec alignment, staged review, targeted validation, and closure. This is useful for turning intentions into a disciplined execution flow with explicit quality gates.

#### plan-checklists
Use when you need explicit plan lifecycle governance instead of ad-hoc notes. It defines where plans are stored, how completion is tracked, and how closure is handled. This is useful for preventing lost context and for making implementation work auditable across sessions.

#### spec-driven-development
Use when repository behavior is ambiguous and desired state should lead implementation. It anchors work to a local `SPEC.md` that captures contracts, ownership, and implementation boundaries. This is useful for improving consistency, reducing accidental rework, and preventing drift between code and intended behavior.

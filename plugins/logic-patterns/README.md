<div align="center">
  <img src="logo.png" alt="logic-patterns logo" width="380"/>
</div>

# logic-patterns

AI reasoning strategies used for structured problem solving and decision quality.

## Skills in this plugin

### 1-3-1
1-3-1 frames a problem, defines constraints and success criteria, then sends three independent proposals through the same brief. It compares those proposals by correctness, simplicity, risk, rollback ease, and execution speed. The main agent selects one path, merges only compatible useful ideas, and exits with a justified implementation plan.

### adversary-loop
Adversary-loop reviews a plan or implementation for correctness, regressions, spec alignment, instruction drift, and weak validation. It fixes only significant valid findings, reruns the smallest relevant checks, and starts a fresh adversarial pass over the updated work. The loop stops only when the latest pass reports no significant findings or only insignificant nits remain.

### bifurcate
Bifurcate splits a complex issue into exactly two competing branches, chooses the highest-leverage branch, and pushes the other onto a decision stack. It recursively repeats that split inside the active branch until it isolates one concrete fix site and validation signal, then implements only that fix. If the original issue-level check still fails, it backtracks to the nearest untried sibling branch and repeats the process until the issue is resolved or all viable branches are exhausted.

### first-principles-rebuild
First-principles-rebuild starts from the repository specification and extracts the fundamental outcomes, invariants, authorities, transitions, and safety boundaries. It removes implementation-shaped assumptions, then inspects the existing system only for constraints that are truly binding. It classifies what to keep, remove, or replace, then hands that simplified target model into a concrete reimplementation plan.

### five-whys
Five-whys investigates the full problem space before asking five explicit why prompts. It answers each why with evidence, resolves or bounds unknowns before moving deeper, and finishes with five `Because` bullets that connect each cause to the prior answer.

### session-review
Session-review turns repeated session discoveries into durable, reusable skill guidance instead of leaving them buried in chat history. It records what lookup or decision path was useful, why it mattered, and whether it belongs in an existing skill or a new narrow skill. It keeps only repeatable patterns and checklists, avoiding hardcoded examples or one-off session details.

### gaslight-loop
Gaslight-loop performs a local self-review over the current diff, latest edits, active plan, or named files. It repeatedly challenges the work with a bug-finding pass, fixes significant issues, and reruns the smallest relevant validation. The loop ends when no significant correctness, regression, omission, spec alignment, or validation findings remain.

### greyhat
Greyhat defines the security review scope, trust boundaries, threat model, sensitive data, and required controls before closing implementation. It attacks authentication, authorization, secrets, injection, SSRF, replay, logging, dependency, and integration surfaces as if they are actively targeted. It fixes high-confidence, high-impact risks first and repeats until no unresolved high or critical security or compliance findings remain.

### make-a-plan
Make-a-plan drafts an implementation plan, then repeatedly asks what major dependency, validation point, failure path, rollback, ownership, observability, or spec detail is missing. It adds each missing detail and repeats until no major gaps remain. It then runs simplification passes that reduce complexity without sacrificing intended outcomes, critical fixes, correctness, or safety.

### multidimensional-planning
Multidimensional-planning expands a broad objective across independent dimensions such as user flows, APIs, architecture, data, operations, security, scale, delivery, and tests. It answers one dimension at a time and immediately turns each answer into concrete plan additions before moving on. It then synthesizes the dimensions into ordered implementation phases, resolves conflicts, and leaves no must-have decision unassigned.

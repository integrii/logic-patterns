---
name: "1-3-1"
description: "Use when a problem needs fast candidate generation and narrowing: run 3 independent fix proposals, compare by constraints/risk/complexity, then execute one selected path."
---

# 1-3-1 strategy

## Goal
Resolve a problem by first understanding it deeply, then collecting multiple candidate fixes, then converging on one decision with the main agent.

## Process
1. **Identify the problem well**
   - Restate the goal and constraints in one sentence.
   - List what is known, unknown, and assumed.
   - Define success criteria and hard constraints.
   - Call out tradeoffs and potential risks before proposing changes.

2. **Fire up 3 subagents**
   - Launch 3 independent subagents on the same problem statement.
   - Each subagent must return:
     - a candidate fix
     - rationale
     - risks
     - expected validation checks
   - Subagents should prefer concrete, minimal, low-complexity approaches.

3. **Main agent selects one fix**
   - Compare the three proposals against the success criteria.
   - Prefer the option with the best risk/reward balance and least complexity.
   - Merge useful parts only if conflicts are resolved explicitly.
   - Choose one final path and execute it.

## Scoring rubric for selection
- Correctness against stated constraints
- Simplicity and maintainability
- Failure-risk and rollback ease
- Execution speed

## Anti-patterns to avoid
- Skipping problem framing and jumping to implementation
- Letting the loudest proposal override evidence
- Combining all proposals without adjudication

## Exit condition
- The main agent has one chosen implementation plan and a short justification tied to the rubric.

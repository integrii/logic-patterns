---
name: five-whys
description: "Use when investigating root cause with the Five Whys method: gather evidence across the full problem space, ask five explicit why questions in sequence, explain each step simply, and return five Because bullets that connect each cause to the prior answer."
---

# Five Whys

## Goal
Find a useful root cause by investigating the problem space first, then asking why five times in a disciplined chain.

## Workflow
1. Investigate the full problem space before asking the five whys.
   - Reproduce or inspect the symptom when feasible.
   - Read relevant code, logs, contracts, docs, metrics, tickets, and recent changes.
   - Separate confirmed facts from hypotheses.
   - Identify the user-visible failure and the system boundary where it appears.
2. Ask why five times as five separate prompts.
   - Write each prompt as `Why 1: Why did <observed problem> happen?`
   - Answer with a short evidence-backed explanation.
   - Use the prior answer as the subject of the next why.
   - Keep the chain causal, not merely chronological.
   - If the answer to a why is unknown, stop and investigate that specific unknown before asking the next why.
   - Use available evidence sources first: code, logs, docs, metrics, runtime state, tickets, traces, and authoritative APIs.
   - Continue to the next why only after the answer is known or the remaining uncertainty is explicit and bounded.
3. Stop after exactly five why prompts.
   - If a branch is weak, say what evidence is missing.
   - Do not keep asking extra whys in the final structure.
4. Return the final causal chain as exactly five bullet points.
   - Each bullet must start with `Because`.
   - Each bullet answers why the bullet above happened.
   - The first bullet answers why the original problem happened.

## Guardrails
- Do not start with guesses. Investigate enough to ground the first why.
- Do not force blame onto people. Prefer process, design, system, observability, and contract causes.
- Do not skip from symptom to broad culture claims.
- Do not treat correlation as cause without evidence.
- Do not hide uncertainty. Mark uncertain links clearly.
- Do not stack a new why on top of an unknown. Resolve or bound the unknown first.

## Output Format
Use this shape:

```text
Why 1: Why did <problem> happen?
<simple explanation>

Why 2: Why did <answer 1> happen?
<simple explanation>

Why 3: Why did <answer 2> happen?
<simple explanation>

Why 4: Why did <answer 3> happen?
<simple explanation>

Why 5: Why did <answer 4> happen?
<simple explanation>

- Because <answer to why 1>.
- Because <answer to why 2>.
- Because <answer to why 3>.
- Because <answer to why 4>.
- Because <answer to why 5>.
```

---
name: five-whys
description: "Use when investigating root cause with the Five Whys method: gather evidence across the full problem space, ask five explicit why questions in sequence, and answer each why with a Because line that connects each cause to the prior answer."
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
4. Return the final causal chain as exactly five why sections.
   - Each section starts with `Why N:`.
   - Each answer line starts with `Because:`.
   - Do not add summary bullets after the five why sections.

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
Because: <answer to why 1>.

Why 2: Why did <answer 1> happen?
Because: <answer to why 2>.

Why 3: Why did <answer 2> happen?
Because: <answer to why 3>.

Why 4: Why did <answer 3> happen?
Because: <answer to why 4>.

Why 5: Why did <answer 4> happen?
Because: <answer to why 5>.
```

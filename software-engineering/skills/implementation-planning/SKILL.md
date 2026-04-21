---
name: implementation-planning
description: "Produce a constrained implementation plan (goal, first test, further test cases) before writing any code. Apply when asked to implement a feature or start a ticket."
---

# Implementation Planning

When asked to implement anything, produce a plan before writing code. Plans are deliberately constrained.

## Trigger conditions

Apply this skill when:
- Asked to implement a feature or behaviour
- Asked for a plan before coding
- Starting work on a new ticket or issue

## A plan has three parts

1. **Goal** — one or two sentences in domain language describing the behaviour being added
2. **First test** — fully specified: suite name, test name, setup (Given builders), action, assertion
3. **Further test cases** — high-level list only; hypotheses, not commitments; one line each with a brief implementation preconception if obvious

## Plan format

```
## Goal
[What behaviour is being added]

## First test
Suite: WhenDoingABehaviour
Test: The_outcome_described_in_plain_english
Setup: [Given builders or state needed]
Action: [What is called]
Assert: [What is expected]

## Further test cases
- [ ] [Behaviour] — [lightweight preconception if relevant]
```

## Rules

- Never plan implementation in detail beyond the first test
- No infrastructure decisions upfront (no MongoDB shape, no API contract, no class structure)
- One plan per concern — multiple small plans over one large one
- Plans are disposable — if the first test proves the plan wrong, replan
- The plan ends when you have enough to write the first test

## What a plan is not

- Not a full technical design
- Not a list of classes and methods to create
- Not a sequence of implementation steps
- Not a guarantee that the further test cases are correct or complete

## Superpowers integration

If the superpowers plugin is installed:
- Invoke `superpowers:brainstorming` before planning any new feature.
- Invoke `superpowers:writing-plans` for multi-step plans — it provides plan format, structure, and review checkpoints.

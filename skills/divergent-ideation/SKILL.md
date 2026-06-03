---
name: divergent-ideation
description: Runs isolated parallel idea generation followed by a separate critic pass for open-ended software engineering decisions. Use when the user asks to brainstorm, compare approaches, name an API/product, explore architecture, investigate fuzzy bugs without a known root cause, or says the obvious answer feels wrong. Do not use for syntax help, factual lookup, known-root-cause bugs, or requests for quick/standard/canonical answers.
---

# Divergent Ideation

Use this skill to avoid premature convergence on the first plausible engineering answer.

The core move, adapted from Udit Akhouri's ADHD skill, is mechanical: first generate ideas in isolated branches under different frames, then run a separate critic pass to cluster, score, prune traps, and deepen the best survivors. Do not mix generation and critique.

## Pre-flight gate

Run this only when the cost is worth it.

Proceed when at least one is true:

- the user explicitly asks for divergent ideation, ADHD-style exploration, brainstorming, or multiple approaches
- the problem is an architecture, API/SDK surface, schema, naming, product design, or strategy decision that will be expensive to reverse
- the bug is fuzzy and the root cause is unknown, so the user needs hypothesis classes rather than a patch
- the user says the obvious answer feels wrong or they are stuck

Abort and answer directly when:

- there is one canonical answer
- the user asks for quick, standard, textbook, canonical, one-line, or just-show-me output
- the issue is a syntax question, factual lookup, or known-root-cause bug
- the answer is one code search or web search away

## Phase 1 — Diverge without critique

1. Restate the problem and the decision to be made in one sentence.
2. Pick 3 to 5 frames from the table below. For code-shaped problems, pick mostly engineering frames plus one wild frame.
3. For each frame, create an isolated branch of reasoning. In environments with a Task/Agent tool, use separate parallel calls. If no such tool exists, simulate the separation by writing each frame's ideas independently and do not let one frame reference another.
4. Each branch must produce 4 to 6 short ideas. No ranking, no critique, no hedging.

Branch instruction:

```text
You are in DIVERGENT mode. Generate 4 to 6 distinct software engineering ideas under this frame. Do not evaluate, rank, hedge, or reuse ideas from other frames. Ban the first three obvious answers. Output short bullets only.
```

## Frames

| Frame | Use it to ask |
| --- | --- |
| On-call engineer | What design prevents 3am pages, noisy alerts, and irrecoverable failure? |
| Hostile competitor / attacker | How would someone exploit, break, game, or embarrass the obvious solution? |
| Hardware / latency engineer | What would memory layout, backpressure, cache locality, or physical limits suggest? |
| Regulator / auditor | What must be provable, reversible, traceable, or explicitly refused? |
| Logistics operator | What would batching, queues, routing, hub-and-spoke, or returns teach us? |
| Game designer | What are the loops, incentives, friction points, save states, and speedrun paths? |
| $0 budget / one hour | What is the crudest tracer bullet that proves the load-bearing thing? |
| Infinite budget / ten years | What maximal version reveals the long-term architecture or platform shape? |
| Remove the assumption | Which fixed constraint might not be fixed: database, framework, request/response, team boundary, or deployment model? |
| Ant colony | How could many small local rules solve this without a central coordinator? |
| Inversion | How would we guarantee failure, then invert those answers into safeguards or approaches? |

## Phase 2 — Focus with a critic

After the divergent branches are complete:

1. Cluster ideas by underlying angle, not surface wording.
2. Score each idea from 0 to 10 on:
   - novelty: is it meaningfully different from the default?
   - viability: could a team actually ship it?
   - fit: does it solve the stated problem?
3. Flag traps: seductive ideas with hidden cost, premature abstraction, migration risk, operational burden, weak security, or bad incentive effects.
4. Shortlist 2 to 4 ideas. Prefer ideas with high viability and fit; keep one non-obvious-but-plausible option when it teaches something.
5. Deepen the top 2 or 3 survivors into implementation sketches with first steps and risks.

## Output shape

Use this structure:

1. **Brief** — problem restatement and frames used.
2. **Wide set** — clusters with idea bullets and score chips like `[N7 V8 F9]`.
3. **Converge** — shortlist with the recommended choice marked clearly; list traps separately.
4. **Focus** — deepened sketches, each with load-bearing risk and first concrete builder step.
5. **Provocation** — one question that opens a useful new direction.

## Engineering guardrails

- Do not present twenty equal options and force the user to choose. Diverge widely, then take a position.
- Do not let wild frames become weird-for-weird's-sake. Each survivor must map to a shippable step.
- Do not use this as an excuse to skip codebase inspection. When claims depend on the actual repo, inspect files before scoring fit or viability.
- Do not preserve all ideas. Pruning is the product.
- When the chosen option implies an architectural decision, propose the ADR boundary that should capture it.

## Attribution

This skill distills the software-engineering-relevant parts of [UditAkhourii/adhd](https://github.com/UditAkhourii/adhd), which is MIT licensed. The adapted concepts are isolated divergent branches, frame-based generation, a separate critic pass, clustering, trap pruning, and deepening top survivors.

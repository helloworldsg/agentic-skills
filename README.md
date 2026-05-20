# Agentic Skills

Opinionated skills for practicing agentic engineering: turning messy software work into clear plans, small issues, and testable implementation loops.

Use this when you want agents to do more than generate code. These skills help engineers specify intent precisely, preserve domain understanding, delegate execution safely, and verify the result through tests, traces, and review.

For the broader thesis, see [The Agentic Blueprint](https://helloworldsg.github.io/agentic-skills/).

## Why Use This

- **Agentic engineering over vibe coding**: replace prompt-and-hope with structured planning, execution, and verification.
- **Less vague planning**: `/plan` stress-tests ideas against the codebase, project vocabulary, and ADRs.
- **Better agent handoffs**: `/plan-to-prd` and `/plan-to-issues` turn context into PRDs and independently grabbable implementation tickets.
- **Smaller, safer delivery**: issues are sliced as tracer bullets, so each one produces a narrow end-to-end path that can be verified on its own.
- **Tests that survive refactors**: `/tdd` focuses on public behavior instead of implementation details.
- **Verification at machine speed**: build around tests, traceable execution, and review gates instead of trusting generated diffs.
- **Architecture that compounds**: `/improve-codebase-architecture` looks for deeper modules with smaller interfaces, better locality, and clearer test surfaces.

## Install

```sh
npx skills@latest add helloworldsg/agentic-skills
```

## Recommended Workflow

```text
/plan -> /plan-to-prd -> /plan-to-issues -> /tdd
```

This maps to the agentic engineering loop:

| Phase | Goal | Skill |
| --- | --- | --- |
| Plan | Define intent, domain language, constraints, and architectural boundaries. | `/plan` |
| Execute | Convert shared context into PRDs and vertical-slice issues that agents can work on independently. | `/plan-to-prd`, `/plan-to-issues` |
| Verify | Implement one behavior at a time and prove it through public interfaces. | `/tdd` |

The point is not to outsource understanding. The engineer still owns taste, architecture, evaluation, and security posture. The agent handles more of the execution loop.

## Skills

| Skill | Use it for |
| --- | --- |
| `/plan` | Challenge a plan, clarify domain terms, and capture glossary or ADR updates as decisions crystallize. |
| `/plan-to-prd` | Convert the current context into a PRD that an implementation agent can use. |
| `/plan-to-issues` | Break a plan or PRD into small, dependency-aware vertical-slice issues. |
| `/tdd` | Build features or fix bugs one behavior at a time with tests through public interfaces. |
| `/improve-codebase-architecture` | Find refactoring opportunities that make modules deeper, more testable, and easier for agents to navigate. |
| `/create-skill` | Create new skills with clear triggers, structure, and supporting resources. |
| `/caveman` | Switch to an ultra-compressed communication style when you want fewer tokens. |

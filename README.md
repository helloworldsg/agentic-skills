# Agentic Skills

Opinionated skills for turning messy software work into clear plans, small issues, and testable implementation loops.

Use this when you want an agent to do more than write code: sharpen domain language, record important decisions, break work into vertical slices, and implement with behavior-first tests.

## Why Use This

- **Less vague planning**: `/plan` stress-tests ideas against the codebase, project vocabulary, and ADRs.
- **Better agent handoffs**: `/plan-to-prd` and `/plan-to-issues` turn context into PRDs and independently grabbable implementation tickets.
- **Smaller, safer delivery**: issues are sliced as tracer bullets, so each one produces a narrow end-to-end path that can be verified on its own.
- **Tests that survive refactors**: `/tdd` focuses on public behavior instead of implementation details.
- **Architecture that compounds**: `/improve-codebase-architecture` looks for deeper modules with smaller interfaces, better locality, and clearer test surfaces.

## Install

```sh
npx skills@latest add helloworldsg/agentic-skills
```

## Recommended Workflow

```text
/plan -> /plan-to-prd -> /plan-to-issues -> /tdd
```

1. Use `/plan` to clarify the domain language, constraints, and decisions.
2. Use `/plan-to-prd` to turn the conversation into a PRD.
3. Use `/plan-to-issues` to break the PRD into vertical slices.
4. Use `/tdd` to implement each slice with a red-green-refactor loop.

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


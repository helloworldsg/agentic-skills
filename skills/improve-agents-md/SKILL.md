---
name: improve-agents-md
description: Improve AGENTS.md, CLAUDE.md, and agent rule files by making them concise, token-efficient, verifiable, and easier for coding agents to follow.
---

# Improve Agent Instructions

Goal: compress agent instructions without losing project-specific guidance. Target `AGENTS.md` near 2.5k tokens unless the repo truly needs more.

## Workflow

1. Inspect current instructions and nearby source-of-truth files:
   - `wc -w AGENTS.md CLAUDE.md 2>/dev/null`
   - `rg "test|lint|format|build|typecheck" package.json pyproject.toml Makefile justfile .github -g '!node_modules'`
2. Delete low-value text:
   - generic agent advice, tool tutorials, obvious git/npm/bun explanations
   - duplicated commands already listed in package scripts or task runners
   - long rationale, history, examples, or policy prose that does not guide action
3. Keep only operational project knowledge:
   - project purpose, key paths, architecture boundaries
   - essential commands and verification gates
   - non-obvious conventions, constraints, failure modes
   - dated learnings from real mistakes
4. Rewrite rules as checkable patterns.

## Preferred Shape

```markdown
# AGENTS.md

## Overview
1-2 sentences.

## Structure
Key paths only.

## Commands
Essential commands only.

## Workflow
Project-specific constraints.

## Rules
Compressed, verifiable rules.

## Learnings
- YYYY-MM-DD: Concrete lesson from an actual issue.
```

## Rule Format

```markdown
**Rule name** - Specific instruction with a tiny example.
*Verify:* Command, search, test, or inspection step.
*Fix:* Exact remediation.
```

Example:

```markdown
**Types over interfaces** - Use `type User = {}` instead of `interface User {}`.
*Verify:* `rg "interface [A-Z]" src`
*Fix:* Convert object interfaces to type aliases.
```

## Final Check

- `wc -w` improved meaningfully; rough tokens = words x 1.3
- no commands duplicated from package/task files without added context
- every durable rule has a verification path
- learnings are dated and based on observed issues, not guesses

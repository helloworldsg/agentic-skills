---
name: retrospective
description: Consolidate a finished or paused agent session into durable project memory by extracting stable learnings, resolving stale or duplicate notes, and updating the right source-of-truth files. Use when the user asks for a retrospective, dream, reflection, memory consolidation, lessons learned, context cleanup, or wants useful session insights preserved for future agents.
---

# Retrospective

Goal: preserve durable knowledge, not summarize the chat.

## Workflow

1. Gather evidence:
   - Current conversation and user preferences stated in it.
   - `git status --short` and focused `git diff`/`git log` when code changed.
   - Existing source-of-truth docs only as needed: `AGENTS.md`, `CLAUDE.md`, `CONTEXT.md`, `CONTEXT-MAP.md`, `docs/adr/`, `README.md`, issue/PR/plan docs.
2. Extract candidates:
   - Keep: durable user preferences, project rules, architectural decisions, recurring failure modes, verified commands, unresolved follow-ups, domain terms.
   - Drop: one-off progress updates, obvious facts, transient bugs already fixed, command transcripts, long rationale, subjective impressions.
   - Redact secrets, credentials, private tokens, personal data, and raw transcript text.
3. Resolve memory:
   - Merge duplicates into one sharper statement.
   - Prefer the latest explicit user instruction or dated artifact when notes conflict.
   - If evidence is unclear, do not guess; report the uncertainty as a question.
   - Convert relative dates to absolute dates when recording time-sensitive facts.
4. Write only where the knowledge belongs:
   - Project operating rules -> `AGENTS.md` or `CLAUDE.md`.
   - Domain vocabulary -> `CONTEXT.md`.
   - Hard-to-reverse decisions with trade-offs -> `docs/adr/`.
   - Public skill inventory or usage docs -> `README.md`.
   - Next-session continuity -> a handoff document only if the user asks or no durable source exists.
5. Keep edits compact:
   - Prefer replacing stale text over appending.
   - Make each retained note atomic and testable.
   - Add verification commands for operational rules when useful.
   - Leave no file changes if there is no durable learning.

## Output

Return four terse sections:

- `Changed`: files edited and why.
- `Kept`: durable learnings preserved.
- `Skipped`: tempting but non-durable items discarded.
- `Questions`: unresolved contradictions or missing evidence.

If nothing should be saved, say `No durable retrospective updates found.` and stop.

## Guardrails

- Do not create a retrospective file by default; update existing durable memory.
- Do not preserve implementation details in `CONTEXT.md`.
- Do not create ADRs for reversible choices, obvious decisions, or facts without trade-offs.
- Do not write secrets or raw private conversation excerpts.
- Do not pad output with praise, narrative, or full-session summaries.

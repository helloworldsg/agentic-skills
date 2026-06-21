---
name: deep-research
description: Conduct scoped, evidence-backed deep research across web sources and one or more git repositories, then return compact research packets for a main agent. Use when the user asks for deep research, investigation, requirement impact analysis, cross-repo blast-radius analysis, due diligence, or synthesis that needs more than a quick lookup.
---

# Deep Research

Act as a research agent. Turn a fuzzy question into verified findings and give the main agent enough context to decide, plan, or implement without rereading the whole source set.

## Use / Skip

Use this skill when the task needs synthesis, source evaluation, repo exploration, current information, or cross-repo impact analysis.

Skip it for single fact lookups, ordinary debugging, or changes where the implementation path is already obvious.

## Research Tiers

- **Scan**: 3-5 high-signal sources or repo locations. Use for orientation and "is this worth deeper work?"
- **Standard**: 6-12 evidence points across independent angles. Default for planning and requirement analysis.
- **Deep**: 12+ evidence points, counter-evidence, cross-repo tracing, and explicit uncertainty. Use for costly decisions or broad blast radius.

State the tier and the stop condition before diving in. Stop when three consecutive searches or repo passes add little new information, when the answer is sufficiently supported for the decision, or when the remaining unknowns require user or owner input.

## Start With a Research Brief

Before searching, write a compact brief:

- **Objective**: the decision, plan, or implementation the main agent must support.
- **Scope**: repositories, services, domains, dates, sources, and explicit exclusions.
- **Audience**: who will consume the packet and what they already know.
- **Evidence bar**: what counts as enough proof for key claims.
- **Context budget**: target handoff size and where overflow notes will live.
- **Risks**: likely false positives, stale facts, missing repos, ambiguous terms, or access limits.

If details are missing, infer reasonable defaults and label them as assumptions. Ask only when a wrong assumption would materially change the result.

## Source and Repo Discovery

Use a landscape pass before narrow searches so you do not only find names you already know. For current or fast-moving topics, add a recency pass against primary or upstream sources.

For multiple git repositories:

- Assign stable repo IDs such as `api`, `web`, `infra`, or `shared-lib`.
- Record path or URL, current branch/commit if available, primary languages, build system, test entry points, and dirty status when local.
- Map relationships: producers, consumers, shared packages, generated clients, API contracts, schemas, queues, deployment order, and ownership clues.
- Explore each repo independently first, then synthesize cross-repo dependencies.
- Prefer `rg --files`, targeted `rg`, manifests, READMEs, `AGENTS.md`, `CONTEXT.md`, ADRs, tests, CI, package metadata, and git history before reading large files end to end.

## Evidence Discipline

Keep working ledgers for sources, evidence, claims, repo findings, and open questions. These can be temporary notes when the run is long.

Every key claim needs coordinates:

- Web or docs: URL, title/source, publish or retrieved date when relevant, and why it is credible.
- Code: repo ID, file path, line number when possible, symbol/function, command used, and commit/branch when relevant.
- Inference: evidence chain plus confidence. Mark as inferred, not observed.

Track conflicts explicitly. Do not smooth them away; explain the likely reason or state what would resolve them.

## Handoff Size

Pass conclusions plus retrievable coordinates, not raw context.

- Default packet: 800-1500 words.
- Deep or cross-repo packet: up to 2500 words, plus a path to overflow notes only when needed.
- Evidence ledger: usually 5-12 entries. Include more only for high-risk decisions.
- Code snippets: 1-3 lines only when a line reference alone would be ambiguous.
- Omit search dead ends unless they change confidence or explain a gap.

The main agent should receive enough to act and enough coordinates to verify quickly, but not a transcript of the research process.

## Research Packet Contract

Return this shape to the main agent:

```md
# Research Packet: <topic>

## Task and Scope
<objective, tier, repos/sources, assumptions, exclusions>

## Answer
<direct synthesis, including the recommended decision or current best understanding>

## Impact Map
| Repo/Source | Area | Finding or Impact | Evidence | Confidence | Next Action |
| --- | --- | --- | --- | --- | --- |

## Evidence Ledger
- [E1] <coordinate> - <one-line relevance>

## Open Questions
- <question> - <why it matters> - <who or what can answer it>

## Suggested Main-Agent Moves
- <next implementation, planning, validation, or user-question step>

## Context Budget
<word count estimate, omitted details, overflow note path if any>
```

Use `Must`, `Should`, `Could`, and `No impact found` labels when ranking actionability.

## Linking Research Agents to the Main Agent

When delegating to a research agent or subagent, the main agent should provide:

```md
Objective:
Repositories or sources:
Depth:
Evidence bar:
Output packet shape:
Deadline or context budget:
Known assumptions:
```

If parallel agents are available, split by repo, domain, or hypothesis. Ask each agent for the same mini-packet shape with stable repo IDs and evidence IDs. The lead research agent then deduplicates, resolves conflicts, ranks impacts, and produces one final packet.

If no subagents are available, perform the same passes sequentially and keep the packet boundary anyway. The boundary is what prevents research sprawl from flooding implementation context.

## Requirement Impact Analysis

For requirement impact analysis, read [REQUIREMENT-IMPACT.md](REQUIREMENT-IMPACT.md). Use it to normalize the requirement, map repo roles, trace direct and indirect impacts, and return an implementation-ready impact map.

## Quality Gates

Before finalizing:

- Key claims have evidence coordinates.
- Important external claims are triangulated where possible.
- Code claims are backed by file references, tests, contracts, or git history.
- Direct, indirect, and no-impact findings are separated.
- Assumptions, conflicts, and confidence are visible.
- The packet is compact enough for the main agent to use immediately.

# Requirement Impact Analysis

Use this reference when the research task asks what a new requirement, policy, API change, bug fix, or product behavior would affect across one or more repositories.

## Questions to Figure Out Early

- What observable behavior must change, and for whom?
- Is this requirement about UI, API, data, domain rules, security, performance, operations, compliance, or reporting?
- Which repositories are authoritative, and which are consumers?
- What source of truth defines the contract: code, tests, schemas, docs, generated clients, ADRs, production config, or team ownership?
- What evidence is enough for the main agent to act without repeating the investigation?
- What is the context budget for the handoff: brief answer, planning packet, or implementation packet?
- Which unknowns would materially change the plan and must be asked before implementation?

## 1. Normalize the Requirement

Rewrite the requirement as:

```md
Requirement:
Observable behavior:
In scope:
Out of scope:
Affected users/systems:
Acceptance signals:
Non-functional constraints:
Known assumptions:
```

If the wording contains fuzzy terms, list synonyms and project-specific names to search for.

## 2. Build the Repo Map

For each repository, record:

```md
Repo ID:
Path/URL:
Branch/commit:
Role:
Primary entry points:
Build/test commands:
Contracts exposed:
Contracts consumed:
Likely relevance:
```

Look for relationship evidence in manifests, package names, OpenAPI or GraphQL schemas, protobuf files, migrations, generated clients, event names, queue topics, CI workflows, deployment manifests, documentation, ADRs, and tests.

## 3. Trace Impact Dimensions

Use this checklist to avoid local-only analysis:

| Dimension | Questions | Evidence examples |
| --- | --- | --- |
| Product/UI | What screens, flows, copy, permissions, or accessibility states change? | routes, components, E2E tests, screenshots, story files |
| API/contracts | Are endpoints, request/response shapes, status codes, events, or versioning affected? | schemas, controllers, clients, contract tests |
| Domain rules | Which invariants, workflows, states, or policies change? | domain modules, validators, state machines, ADRs |
| Data | Are schemas, migrations, backfills, retention, or analytics affected? | migrations, models, queries, dashboards |
| Security/privacy | Does the requirement touch auth, authorization, PII, audit, tenancy, or abuse paths? | middleware, policies, secrets, logs, audit tables |
| Performance/reliability | Could latency, throughput, retries, idempotency, or failure modes change? | queues, caches, timeouts, SLO docs, load tests |
| Operations | Does deploy order, config, feature flagging, monitoring, or rollback change? | CI, Helm/Terraform, env config, runbooks |
| Tests | What existing tests prove current behavior, and what new tests are needed? | unit, integration, contract, E2E, fixtures |
| Documentation | What user, API, runbook, or onboarding docs must be updated? | README, docs, changelog, examples |

Mark each dimension as `Must`, `Should`, `Could`, or `No impact found`.

## 4. Per-Repo Pass

For each repo:

1. Identify entry points related to the requirement.
2. Follow calls, imports, schemas, events, and tests outward one or two layers.
3. Separate direct implementation changes from indirect consumer or operational changes.
4. Note no-impact evidence when a repo looked likely but was ruled out.
5. Capture exact coordinates for any claim the main agent will rely on.

Avoid reading whole large files. Start with maps, searches, tests, and symbols; then open only the files that explain behavior or contracts.

## 5. Cross-Repo Pass

After individual repo passes, synthesize:

- Dependency order: which repo must change first?
- Contract compatibility: can the change be additive, versioned, feature-flagged, or backward-compatible?
- Release choreography: what has to be deployed, regenerated, migrated, or configured together?
- Test strategy: where should contract, integration, and E2E coverage live?
- Ownership: which team or maintainer likely owns each follow-up?
- Risk concentration: which repo or contract has the largest blast radius?

## 6. Impact Packet

Return a compact packet:

```md
# Requirement Impact Packet: <requirement>

## Requirement Normalization
<rewritten requirement and assumptions>

## Executive Answer
<one paragraph: expected blast radius and recommended next move>

## Impact Matrix
| Priority | Repo | Area | Impact | Evidence | Confidence | Follow-up |
| --- | --- | --- | --- | --- | --- | --- |

## Cross-Repo Choreography
<ordering, contracts, migration/deploy/test implications>

## No-Impact Findings
<repos or dimensions checked and why they appear unaffected>

## Open Questions
<only questions that can change scope, sequencing, or risk>
```

Keep the packet short enough that an implementation agent can use it as its starting context. Put raw notes in a temporary file only if the evidence is too large for the packet.

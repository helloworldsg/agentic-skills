---
name: agentic-planning
description: Comprehensive agentic planning skill synthesizing best practices from Hermes Agent, Oh My OpenAgent, Skills.sh, CrewAI, LangGraph, OpenHands, MetaGPT, Aider, Anthropic CLI, AutoGPT, and AgentProtocol. Covers plan-before-execute, ambiguity reduction, task decomposition, review loops, error recovery, and wisdom accumulation.
version: 1.0.0
author: Hermes Agent (research synthesis)
license: MIT
metadata:
  hermes:
    tags: [planning, ambiguity-reduction, task-decomposition, agentic-workflows, implementation, design]
    related_skills: [writing-plans, subagent-driven-development, test-driven-development, plan]
    sources:
      - NousResearch/hermes-agent — plan skill, iteration budgets, skill system, progressive disclosure, prompt stability
      - code-yeongyu/oh-my-openagent — Prometheus/Metis/Momus 3-layer planning, IntentGate, wisdom accumulation
      - vercel-labs/skills (Skills.sh) — SKILL.md format, skill discovery, quality gates
      - CrewAI — AgentPlanner, expected_output, process patterns
      - LangGraph — 5-step thinking, typed state, error categorization, interrupt()
      - OpenHands — stateless RA-loop, security gating, context condensation, sub-agent delegation
      - AutoGPT — block composition, AgentProtocol task-step API
      - Aider — architect/coder separation, CONVENTIONS.md
      - MetaGPT — SOP-driven multi-agent, role-document-role handoff
      - AgentProtocol — universal task-step execution standard
---

# Agentic Planning — Best Practices Synthesis

Distilled from 11 agentic frameworks and production agent code.

**Core axiom:** *Ambiguity is reduced by making implicit knowledge explicit before execution begins.
* Every framework that succeeds at complex agent tasks has a mechanism for externalizing what would otherwise remain tacit assumption.

---

## When to Use

**Always use when:**
- Implementing multi-step features or complex requirements
- Breaking down ambiguous or underspecified tasks
- Delegating work to subagents or other agents
- Starting any task where "what done looks like" is not crystal clear

**Never skip when:**
- Feature seems simple (assumptions cause bugs)
- You plan to implement it yourself (future-you needs guidance)
- Working alone (documentation matters)
- The request is vague, open-ended, or has multiple valid interpretations

---

## The 7-Phase Planning Protocol

### Phase 1: Intent Classification (from Oh My OpenAgent IntentGate)

Before planning, classify the task intent. Different intents require different planning styles:

| Intent | Planning Focus | Key Questions |
|--------|---------------|---------------|
| **Research** | Discovery scope, source quality, synthesis format | What do we already know? What gaps exist? |
| **Implementation** | Architecture, file paths, testing, acceptance criteria | What exists already? What is the minimal viable change? |
| **Investigation** | Reproduction steps, hypothesis list, evidence collection | What are the possible causes? How do we verify? |
| **Fix** | Root cause analysis, regression risk, rollback strategy | What tests verify current behavior? What is the blast radius? |
| **Refactoring** | Behavior preservation, incremental steps, test coverage | What tests exist? Can we make changes incrementally? |

**Action:** State the classified intent at the top of the plan. This prevents misapplying a research planning style to a fix task (or vice versa).

---

### Phase 2: Requirement Extraction

**Never accept vague instructions.** Use intent-specific extraction:

**For implementation/scratch-builds:**
- What patterns exist in the codebase? Follow or deviate?
- What is the minimal viable scope? (YAGNI)
- What are the acceptance criteria?
- What edge cases exist?

**For fixes/refactoring:**
- What tests verify current behavior?
- What is the rollback strategy?
- What is the blast radius of the change?

**For ambiguous requests:**
- Use `clarify` to ask intent-specific questions
- List assumptions explicitly in the plan
- Mark assumptions with `[ASSUMPTION]` for visibility

**Key principle (from MetaGPT):** Each role in the process produces a *structured document* with a defined schema. The plan is that document. Never proceed with "I will figure it out as I go."

---

### Phase 3: Gap Analysis (from Oh My OpenAgent Metis)

Before finalizing the plan, run an explicit gap check:

- [ ] Are there hidden intentions not captured in writing?
- [ ] Are there AI-slop patterns (over-engineering, scope creep)?
- [ ] Are acceptance criteria missing or vague on any task?
- [ ] Are there edge cases not addressed?
- [ ] Are there assumptions that should be verified?
- [ ] Is any task more than 5 minutes of focused work? (If yes, decompose further)

**Key insight:** "The plan author has ADHD working memory — it makes connections that never make it onto the page. Metis forces externalization of implicit knowledge."

---

### Phase 4: Plan Document Generation

#### Plan Structure

```markdown
# [Feature Name] Implementation Plan

> **Intent:** [research|implementation|investigation|fix|refactoring]

**Goal:** [One sentence — what this builds/achieves]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

**Assumptions:**
- [ASSUMPTION] [description] — [how to verify]

**Risks and Tradeoffs:**
- [RISK] [description] — [mitigation]

**Open Questions:**
- [ ] [question] — [who to ask / how to resolve]

---

## Task List

### Task 1: [Descriptive Name]

**Objective:** [What this accomplishes — one sentence]
**Done when:** [Concrete, measurable acceptance criteria]
**Files:**
- Create: `exact/path/to/new_file.py`
- Modify: `exact/path/to/existing.py:45-67`
- Test: `tests/path/to/test_file.py`

**Step 1: [Action]**
[Exact code or command — copy-pasteable]

**Step 2: [Action]**
[Exact code or command — copy-pasteable]

**Expected output:** [What success looks like — exact]

**Rollback:** [How to undo if something goes wrong]

---

### Task N: [...]
[Same structure]
```

#### Task Granularity Rules

**Each task = 2-5 minutes of focused work.** (from Hermes writing-plans)

| Vague (bad) | Precise (good) |
|-----------|-------------|
| "Build authentication system" | "Create User model with email field" |
| "Add validation" | "Add email format validation to User.email" |
| "Write tests" | "Write test: User with invalid email raises ValueError" |

**Decomposition test:** Can you describe the task in a single verb phrase? If not, it is too big.

---

### Phase 5: Plan Review (from Oh My OpenAgent Momus)

Review the complete plan against these 4 quantified criteria:

| Criterion | Threshold | How to Check |
|-----------|-----------|-------------|
| **Clarity** | 100% of tasks specify WHERE to find implementation details | Every task has exact file paths |
| **Verification** | 90%+ of tasks have concrete, measurable acceptance criteria | Every task has "Done when:" |
| **Context** | Sufficient to proceed without more than 10% guesswork | No task requires business-logic assumptions |
| **Big Picture** | Purpose/background/workflow clear from reading the plan alone | Someone with zero context can understand the plan |

**Only approve when ALL thresholds are met.** If rejected, revise and resubmit — no retry limit.

---

### Phase 6: Error Pre-Classification (from LangGraph)

For each task, classify potential errors and pre-define recovery:

| Error Type | Examples | Recovery Strategy |
|-----------|---------|-----------------|
| **Transient** | Network timeout, rate limit, 429 | Auto-retry with exponential backoff |
| **LLM-recoverable** | Parse failure, wrong tool args | Store error in state, loop back so LLM adjusts |
| **User-fixable** | Missing API key, ambiguous requirement | `interrupt()` — wait for human input |
| **Unexpected** | Unhandled exception, segmentation fault | Bubble up, log full context, halt |

**Add this to the plan when tasks involve:** API calls, file I/O, external services, ambiguous user input, or multi-step transformations.

---

### Phase 7: Execution with Wisdom Accumulation (from Oh My OpenAgent)

During execution, accumulate learnings for future tasks and sessions:

**After each task completes, extract:**

| Category | What to Record |
|----------|---------------|
| **Conventions** | Patterns discovered (e.g., "This project uses factory pattern for models") |
| **Successes** | What worked well (e.g., "Running tests before committing caught a bug") |
| **Failures** | What went wrong (e.g., "Assumed PostgreSQL but project uses SQLite") |
| **Gotchas** | Non-obvious traps (e.g., "Config must be reloaded after env change") |
| **Commands** | Useful commands discovered (e.g., "make test-single TEST=test_name") |

**Write learnings to:** the plan document under a Wisdom section, OR to project-level convention files, OR to Hermes memory — whichever is most durable for future sessions.

---

## Core Principles (Synthesized)

### 1. Plan-Before-Execute (CrewAI + Aider + AutoGPT)
Always produce a plan artifact before acting. Even a simple numbered list of steps catches 80% of misunderstandings early. Never execute on the first turn if the task has more than 1 step.

### 2. Define "Done" Explicitly (CrewAI expected_output)
The single most effective ambiguity reduction technique. Every task must state what success looks like in concrete, measurable terms. If you cannot define "done," the task is too ambiguous to start.

### 3. Separate Planning from Execution (Aider architect mode + MetaGPT SOPs)
Planning uses the strongest reasoning; execution can use faster models. The planner should be READ-ONLY (create markdown only). The executor should be ACTION-ONLY (edit code, run commands). Never both in the same agent turn.

### 4. Make Implicit Knowledge Explicit (Skills.sh + Aider + OpenHands)
Convention files, skills, and SOPs are loaded BEFORE execution. Do not make agents discover project conventions through trial and error — externalize them upfront.

### 5. Typed State, Not Free-Form Text (LangGraph + OpenHands)
Define what data flows between tasks using explicit schemas. Distinguish "not set" from "empty." Store raw data, format prompts on-demand. Different consumers need the same data formatted differently.

### 6. Bite-Sized Decomposition (Hermes + AgentProtocol)
Each task = 2-5 minutes of focused work. If a task exceeds its iteration budget, it must be broken down further. The AgentProtocol enforces this at the API level (task then steps).

### 7. Quality Gates, Not Trust (Skills.sh + OpenHands + Momus)
Every external input is verified (skill quality gates: install count, reputation, stars). Every plan passes review (Momus: 4 quantified criteria). Every action is security-rated (OpenHands: Low/Med/High risk). Never trust unvetted inputs.

### 8. Progressive Disclosure (Hermes skill system)
Load context on-demand, not upfront. Skills use 3-level loading: name+description then full SKILL.md then reference files. This saves tokens and prevents context flooding.

### 9. Interruptible Execution (Hermes + OpenHands + LangGraph)
HITL interrupts are first-class, not afterthoughts. When ambiguity is encountered mid-execution, interrupt and wait for human input rather than guessing. User can redirect mid-task without corrupt state.

### 10. Prompt Stability (Hermes core principle)
System prompt and loaded skills do not change mid-conversation. No cache-breaking mutations except explicit user actions. This eliminates ambiguity about what instructions the agent is following.

---

## Common Anti-Patterns to Avoid

| Anti-Pattern | Why It Fails | Fix |
|-------------|-------------|-----|
| **Vague task names** | "Add authentication" — implementer guesses scope | "Create User model with email and password_hash fields" |
| **Missing acceptance criteria** | No way to know if task is done | Every task has "Done when:" with measurable criteria |
| **Assumed context** | Implementer does not know project conventions | Load convention files; include conventions in plan |
| **God tasks** | "Build the whole feature" as a single item | Decompose into 2-5 minute atomic steps |
| **No rollback plan** | Failures cascade with no recovery | Every risky task has a rollback/undo procedure |
| **Free-form inter-task communication** | "Pass the data from task 3 to task 7" | Define typed schemas for task outputs |
| **Planning + execution mixed** | Planner starts coding; coder starts redesigning | Strict separation: planner = read-only, executor = action-only |
| **No error recovery** | First error halts everything | Pre-classify errors with recovery strategies |
| **Siloed learnings** | Same mistake repeated in next task/session | Accumulate wisdom; write to convention files or memory |
| **Over-engineering (YAGNI)** | "Add flexibility for future requirements" | Implement only what is needed now |

---

## Plan Document Checklist

Before finalizing, verify:

- [ ] Intent is classified and stated at the top
- [ ] Goal is one sentence
- [ ] Assumptions are listed with `[ASSUMPTION]` tags and verification methods
- [ ] Risks and tradeoffs are identified with mitigations
- [ ] Open questions are listed with resolution paths
- [ ] Every task has exact file paths (not "the config file" but `src/config/settings.py`)
- [ ] Every task has concrete "Done when:" acceptance criteria
- [ ] Every task is 2-5 minutes of focused work
- [ ] Code examples are complete and copy-pasteable
- [ ] Commands are exact with expected output
- [ ] Error-prone tasks have pre-classified recovery strategies
- [ ] Risky tasks have rollback procedures
- [ ] DRY, YAGNI, TDD principles applied
- [ ] Plan passes Momus-style 4-criteria review (Clarity 100%, Verification 90%+, Context 10% or less guesswork, Big Picture clear)
- [ ] Wisdom accumulation strategy is defined (where to record learnings)

---

## Integration with Hermes Workflow

### With writing-plans skill
This skill **supersedes and extends** writing-plans. The writing-plans format is used as the base plan document structure, augmented with:
- Intent classification (Phase 1)
- Gap analysis (Phase 3)
- Plan review criteria (Phase 5)
- Error pre-classification (Phase 6)
- Wisdom accumulation (Phase 7)

### With subagent-driven-development skill
After plan is approved:
1. Save plan to `.hermes/plans/YYYY-MM-DD_HHMMSS-<slug>.md`
2. Execute using subagent-driven-development: fresh delegate_task per task
3. After each task: spec compliance review then code quality review then wisdom extraction
4. Proceed only when both reviews approve

### With test-driven-development skill
Every code-producing task in the plan follows TDD:
1. Write failing test
2. Run to verify failure
3. Write minimal code
4. Run to verify pass
5. Commit

### With Hermes memory
After complex plans (5+ tasks), save durable learnings:
- Project conventions discovered — memory (user or memory store)
- Reusable workflows — skill_manage (create/update skills)
- Environment facts — memory

---

## Rapid-Planning Shortcut (For Time-Boxed Situations)

When you need a plan fast (under 1 minute) but still want the core safeguards:

1. **State the intent** (1 line)
2. **List 3-5 assumptions** (bullets)
3. **Define "done"** (1 line per task)
4. **Number the steps** (1 verb phrase each)
5. **Identify the 1 top risk** (1 line)

This covers the 80/20: intent classification, assumption surfacing, acceptance criteria, decomposition, and risk awareness — in under 60 seconds.

---

## Sources

- **Hermes Agent** — plan skill, iteration budgets, progressive disclosure, prompt stability principle
- **Oh My OpenAgent** — Prometheus strategic planner, Metis gap analyzer, Momus ruthless reviewer, IntentGate, wisdom accumulation
- **Skills.sh** — SKILL.md format, find-skills 6-step flow, quality gates
- **CrewAI** — AgentPlanner, expected_output, process patterns (Sequential/Hierarchical)
- **LangGraph** — 5-step thinking framework, TypedDict state, error categorization, interrupt()
- **OpenHands** — Stateless RA-loop, SecurityAnalyzer, context condensation, sub-agent delegation
- **Aider** — Architect/coder separation, CONVENTIONS.md, architect mode
- **MetaGPT** — SOP-driven multi-agent, role-document-role handoff chains
- **AutoGPT** — Block composition, AgentProtocol task-step API
- **AgentProtocol** — Universal task-step execution standard (OpenAPI spec)
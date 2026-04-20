---
name: writing-test-spec
description: Generates test specifications from feature specifications using best practices synthesized from Hermes Agent, Oh My OpenAgent, Anthropic Skills, OpenAI Codex, Aider, Google Testing, and BDD/SBE/ATDD methodologies. Covers 7 core spec→test patterns, unified pipeline, optimal spec structure, and quality gates.
version: 1.0.0
author: Hermes Agent (research synthesis)
license: MIT
metadata:
  hermes:
    tags: [testing, test-specification, spec-to-test, BDD, TDD, test-generation, feature-specs]
    related_skills: [agentic-planning, test-driven-development, writing-plans]
    sources:
      - NousResearch/hermes-agent — design principles as test axioms, execution flow as test paths, explicit test deletion lists, hermetic testing
      - code-yeongyu/oh-my-openagent — Given/When/Then grammar, factory pattern = testable, lifecycle diagrams → describe blocks, auto-regenerated specs
      - Anthropic Skills — expectations schema with grading, baseline comparison, non-discriminating test detection, evals.json
      - OpenAI Codex — SKILL.md + AGENTS.md, sandbox-aware test execution
      - Aider — exercism exercises (tests ARE spec), benchmark runner, tests as irreversible specification
      - Google Testing Philosophy — test sizes (small/medium/large), Beyoncé Rule, 80/15/5 pyramid
      - BDD/SBE/ATDD — Given/When/Then, Example Mapping, ubiquitous language, specs as single source of truth
---

# Writing Test Specifications — Best Practices Synthesis

Distilled from 7+ agentic frameworks and established software engineering methodologies.

**Core axiom:** *Tests derived from design principles survive refactors; tests derived from implementation details do not.* Every successful framework generates tests from spec intent, not from code structure.

---

## When to Use

**Always use when:**
- Writing feature specifications that need test coverage
- Generating tests from specs for new or existing features
- Converting vague requirements into verifiable test cases
- Planning test suites with proper size/scope classification

**Never skip when:**
- Feature spec mentions design principles (principles = test axioms)
- Multi-step execution flows exist (each step = test paths)
- Lifecycle/state transitions are defined (each transition = describe block)
- Simplifying/removing features (deletion lists prevent test rot)

---

## The 7 Core Patterns for Spec→Test Generation

### Pattern 1: Design Principles as Test Axioms (Hermes Agent)

Each numbered design principle in the spec becomes one or more named test cases.

```
Spec says:
  Principle 1: "Let it crash. No silent fallbacks."
  Principle 4: "os.execvp on the happy path."
  Principle 5: "One human-readable exception for TimeoutExpired."

Tests generated:
  test_get_container_exec_info_crashes_on_permission_error
  test_exec_in_container_calls_execvp
  test_exec_in_container_probe_timeout_prints_message
```

**Why it works:** Design principles are the most durable part of a spec — they encode intent, not implementation. Tests derived from principles survive refactors.

### Pattern 2: Execution Flow as Test Path Map (Hermes Agent)

Each numbered step in the spec's execution flow becomes a testable path with success + failure variants.

```
Spec execution flow:
  Step 1: get_container_exec_info()
  Step 2a: shutil.which(backend) returns None → runtime not found
  Step 2b: Sudo probe succeeds → prefix in exec_cmd
  Step 2b-alt: Sudo probe fails → container-not-running error

Tests generated:
  test_get_container_exec_info_success
  test_exec_runtime_not_found
  test_exec_with_sudo_prefix
  test_exec_container_not_running
```

### Pattern 3: Given/When/Then as Structured Grammar (Oh My OpenAgent, BDD/Gherkin)

Specs are expressed (or parsed into) Given/When/Then structure, creating a formal grammar for test generation.

```typescript
// given
const config: BackgroundTaskConfig = { modelConcurrency: { "anthropic/claude-sonnet-4-6": 5 } }
// when
const limit = manager.getConcurrencyLimit("anthropic/claude-sonnet-4-6")
// then
expect(limit).toBe(5)
```

**Why it works:** Gherkin/Given-When-Then is machine-parseable, enabling automated test scaffolding. Oh My OpenAgent enforces this as a coding convention.

### Pattern 4: Expectations Schema with Grading (Anthropic Skills)

The `evals.json` schema bridges spec intent to test verification:

```json
{
  "skill_name": "my-skill",
  "evals": [{
    "id": "e1",
    "prompt": "actual user request",
    "expected_output": "what should happen",
    "files": ["relevant files"],
    "expectations": [
      {"text": "Tool X was called with correct args", "type": "assertion"},
      {"text": "Output contains Y", "type": "output_check"},
      {"text": "No tool Z was called", "type": "negative_assertion"}
    ]
  }]
}
```

**Key innovation:** Every test MUST run both with-skill and without-skill (baseline). Only tests that discriminate between with/without are valuable — non-discriminating tests are flagged and removed.

### Pattern 5: Factory Pattern = Testable by Construction (Oh My OpenAgent)

Every component uses `createXXX()` factories with explicit inputs/outputs, making test derivation natural:

- `createXXXTool()` → tools with testable inputs
- `createXXXHook(input: PluginInput)` → hooks with injectable config
- `createXXXAgent()` → agents with mockable dependencies

**Why it works:** The factory pattern makes inputs (testable given) and outputs (assertable then) explicit. Tests don't need to reach into internals.

### Pattern 6: Explicit Test Deletion Lists (Hermes Agent)

When features simplify, specs enumerate which tests to DELETE with justification:

```
Tests to delete:
  - test_exec_in_container_tty_retries_on_container_failure
    Reason: "Retry loop removed in new design"
  - test_exec_in_container_propagates_hermes_exit_code
    Reason: "No subprocess.run to check exit codes; execvp replaces process. OS semantics, no test needed."
```

**Why it works:** Prevents test-suite rot, gives reviewers auditability, and forces honest assessment of what's being tested vs. what should be.

### Pattern 7: Lifecycle Diagrams → Test Describe Blocks (Oh My OpenAgent)

Behavioral lifecycle diagrams in specs map directly to test describe blocks:

```
Spec lifecycle:
  LaunchInput → pending → [ConcurrencyManager queue] → running →
  polling → completed/error/cancelled/interrupt

Test structure:
  describe("pending state")     → test queue insertion
  describe("ConcurrencyManager") → test slot acquisition/release
  describe("running → polling")  → test state transition
  describe("completed/error")    → test terminal states
  describe("cancelled/interrupt") → test cleanup flows
```

---

## The Unified Spec→Test Pipeline

```
┌─────────────────────────────────────────────────────────────────┐
│                    FEATURE SPECIFICATION                        │
│  (Goal, Design Principles, Execution Flow, Edge Cases)         │
└──────────────────────┬──────────────────────────────────────────┘
                       │
          ┌────────────▼────────────┐
          │   STEP 1: DECOMPOSE    │  Extract: principles, rules,
          │   (BDD Discovery)      │  examples, edge cases, lifecycle
          └────────────┬───────────┘
                       │
          ┌────────────▼────────────┐
          │   STEP 2: FORMULATE     │  Express each as Given/When/Then
          │   (Gherkin/ATDD)        │  in ubiquitous language (DDD)
          └────────────┬───────────┘
                       │
          ┌────────────▼────────────┐
          │   STEP 3: CLASSIFY      │  Assign test SIZE (small/medium/
          │   (Google Test Sizes)   │  large) and SCOPE (unit/int/E2E)
          └────────────┬───────────┘
                       │
          ┌────────────▼────────────┐
          │   STEP 4: GENERATE      │  Create test specs with:
          │   (Automation)          │  - Explicit test names from principles
          │                        │  - Phase-based test sections
          │                        │  - Line count estimates
          │                        │  - Deletion lists if simplifying
          └────────────┬───────────┘
                       │
          ┌────────────▼────────────┐
          │   STEP 5: ENFORCE       │  Ensure hermeticity:
          │   (Hermetic Testing)    │  - Blank credentials
          │                        │  - Isolate filesystem
          │                        │  - Pin determinism vars
          │                        │  - Mock at boundaries
          └────────────┬───────────┘
                       │
          ┌────────────▼────────────┐
          │   STEP 6: VALIDATE      │  - Baseline comparison (with/without)
          │   (Quality Gate)       │  - Flag non-discriminating tests
          │                        │  - Verify no shortcut-taking
          │                        │  - Confirm assertions match principles
          └────────────┬───────────┘
                       │
          ┌────────────▼────────────┐
          │   STEP 7: ITERATE       │  - Run tests → analyze failures
          │   (Refinement Loop)    │  - Feed failures back to spec
          │                        │  - Improve spec + tests
          │                        │  - Re-run until stable
          └────────────────────────┘
```

---

## Feature Spec Structure That Enables Test Generation

The optimal feature spec structure for test derivation:

```markdown
# Feature: [Name]

## Goal
What this feature accomplishes and why.

## Design Principles
1. [Principle A] — Each becomes one or more test cases
2. [Principle B] — Testable axioms, not vague aspirations
3. [Principle C] — Survives implementation changes

## Behavioral Specification

### Happy Path (Given/When/Then)
- Given [context], When [action], Then [outcome]

### Error Paths
- Given [error context], When [action], Then [error handling per Principle N]

### Edge Cases
- [Edge case 1] → expected behavior
- [Edge case 2] → expected behavior

## Execution Flow
1. [Step 1] → test: test_[step1]_success, test_[step1]_failure
2. [Step 2] → test: test_[step2]_[variant]
3. [Step 3] → test: test_[step3]_completion

## Lifecycle Diagram
[State machine / flow diagram — each transition = a test describe block]

## Test Specification

### Tests to Add (by design principle)
| Test Name | Principle | Type | Size |
|-----------|----------|------|------|
| test_X_crashes_on_Y | Principle 1 | Negative | Small |
| test_Z_calls_execvp | Principle 4 | Positive | Small |
| test_timeout_prints_msg | Principle 5 | Edge | Small |

### Tests to Add (by execution flow)
| Test Name | Flow Step | Variant | Size |
|-----------|-----------|---------|------|
| test_runtime_not_found | Step 2a | Failure | Small |
| test_sudo_prefix_applied | Step 2b | Success | Small |

### Integration Tests
| Test Name | Components | Size |
|-----------|-----------|------|
| test_full_exec_lifecycle | Steps 1-3 | Medium |

### E2E Tests
| Test Name | User Journey | Size |
|-----------|--------------|------|
| test_user_runs_command_end_to_end | Full flow | Large |

### Tests to Delete (if simplifying)
| Test Name | Reason |
|-----------|--------|
| test_old_retry_loop | "Retry loop removed in new design" |

## Phase Plan
| Phase | Scope | New Tests | Test Lines (est.) | Impl Lines (est.) |
|-------|-------|-----------|------------------|------------------|
| 1 | Core | 5 tests | ~150 | ~200 |
| 2 | Gateway | 3 tests | ~100 | ~150 |
| 3 | CLI | 2 tests | ~50 | ~80 |

## Out of Scope
- [Explicitly cannot generate tests for these]

## Rollout Plan
- Phase 1 tests merge with Phase 1 code
- Feature flag: test both enabled/disabled states
```

---

## Key Quality Principles

### From Hermes Agent
- **Specs own the test plan** — Test cases written directly into feature specs, not derived separately
- **Test deletion is first-class** — Explicit deletion lists prevent test-suite rot
- **Hermetic by default** — 5 invariants: blank credentials, isolated home, deterministic runtime, pinned workers, proper venv
- **Phased test dev = phased merges** — Each phase has its own test section with line estimates

### From Oh My OpenAgent
- **Co-located spec+code+test** — AGENTS.md, *.ts, *.test.ts alongside each other
- **Factory pattern = testable** — `createXXX()` factories make inputs/outputs explicit
- **Auto-regenerated specs** — AGENTS.md from code keeps specs in sync
- **Complexity taxonomy drives coverage** — HIGH complexity = 15+ test files; LOW = minimal
- **Architecture enforcement enables testing** — 200 LOC limit, SRP, no catch-all files

### From Anthropic Skills
- **Baseline comparison mandatory** — Every test runs with-skill AND without-skill
- **Non-discriminating test detection** — Flags tests that pass in both baseline and treatment
- **Iterative refinement** — Draft → test → grade → analyze → improve → repeat
- **Programmatic > human grading** — Scripts are "faster, more reliable, reusable across iterations"
- **Expectations schema bridges spec→test** — `{id, prompt, expected_output, expectations[]}`

### From Google's Testing Philosophy
- **Size ≠ Scope** — Test size (small/medium/large by constraints) independent of scope (unit/int/E2E)
- **The Beyoncé Rule** — "If you liked it, then you shoulda put a test on it"
- **Smallest viable test** — Generate the smallest-size test that verifies each assertion
- **80/15/5 pyramid** — Unit / Integration / E2E ratio

### From BDD/SBE/ATDD
- **Specs as single source of truth** — Same examples serve as requirements AND tests
- **Example Mapping** — Systematically discover edge cases via "what if?" questions
- **Declarative > Imperative** — Describe *what* not *how*
- **Ubiquitous language** — Tests use same terms as spec and code

### From AI-Assisted Generation Research
- **Shortcut-taking risk** — LLMs generate superficially plausible tests that don't exercise real behavior
- **RAG grounding essential** — Anchor generation in real codebase, existing tests, domain conventions
- **Verify-then-generate** — Have AI confirm spec understanding before generating tests
- **Decomposition prompting** — "Identify rules, then edge cases, then error conditions, then generate per item"

---

## Anti-Patterns to Avoid

| Anti-Pattern | Source | Why It's Bad |
|-------------|--------|-------------|
| Non-discriminating tests | Anthropic | Tests that pass in both baseline and treatment measure nothing |
| Ice cream cone testing | Google | Many E2E, few unit = slow, flaky, expensive |
| Shortcut-taking | AI Research | Superficially plausible tests that don't exercise specified behavior |
| Hallucinated assertions | AI Research | Assertions that look correct but verify wrong properties |
| Happy-path-only generation | AI Research | Missing edge cases, error conditions, boundary values |
| Stale test accumulation | Hermes | No deletion lists = test suite rots over time |
| Imperative specs | BDD | "Click button X, enter text Y" breaks when UI changes |
| Single-size thinking | Google | All tests at same size level ignores hermeticity/fidelity tradeoff |
| Spec-code-test drift | SBE | When specs, code, and tests evolve independently |

---

## Tooling & Schema Recommendations

### Evaluations Schema (Anthropic Skills — most complete)
```json
{
  "skill_name": "feature-name",
  "evals": [{
    "id": "e1",
    "prompt": "real user request",
    "expected_output": "what should happen",
    "files": ["relevant/files.ts"],
    "expectations": [
      {
        "text": "Verifiable statement about behavior",
        "type": "assertion | output_check | negative_assertion",
        "discriminating": true
      }
    ]
  }]
}
```

### Grading Schema
```json
{
  "expectations": [{"text": "...", "passed": true, "evidence": "..."}],
  "summary": {"passed": 4, "failed": 1, "total": 5},
  "execution_metrics": {"tool_calls": 3, "total_steps": 7, "errors": 0},
  "timing": {"wall_time_s": 12.3, "tokens": 1500},
  "eval_feedback": ["Suggestion for improvement"]
}
```

### Benchmark Schema (for comparing iterations)
```json
{
  "history": [{
    "version": "v1",
    "pass_rate": 0.6,
    "result": "baseline",
    "mean_wall_time": 12.3,
    "mean_tokens": 1500
  }, {
    "version": "v2",
    "pass_rate": 0.8,
    "result": "won",
    "delta_pass_rate": 0.2
  }]
}
```

---

## Practical Recommendations

1. **Start with the spec format** — Use the structure above. Include design principles, execution flow, edge cases, AND test specifications all in one document.

2. **Derive tests from principles, not features** — A feature is a bag of behaviors; a principle is a testable axiom. Generate one or more test cases per principle.

3. **Use Given/When/Then as the grammar** — Whether you generate Gherkin or code-level tests, the Given/When/Then structure provides a formal bridge from spec to test.

4. **Classify every test by size AND scope** — Assign Google's small/medium/large AND unit/integration/E2E independently. Target the 80/15/5 pyramid.

5. **Always test both sides of feature flags** — When a spec is feature-flagged, generate tests for enabled AND disabled states.

6. **Include test deletion lists** — When simplifying features, explicitly enumerate which tests become obsolete with justification.

7. **Require baseline comparison** — For AI-generated tests, always run against a baseline (without the feature/skill). Flag non-discriminating tests.

8. **Use the expectations schema** — The Anthropic `{id, prompt, expected_output, expectations[]}` format is the most complete bridge from spec intent to test verification.

9. **Enforce hermeticity at the infrastructure level** — Blank credentials, isolate filesystem, pin deterministic runtime vars. Make it impossible to write non-hermetic tests.

10. **Iterate in phases** — Break test development into phases matching implementation. Each phase merges independently with its own test section and line estimates.

---

## Integration with Related Skills

### With agentic-planning skill
When planning features, include the test specification structure from this skill in the plan document. The plan's task list should reference specific test names from the spec's Test Specification section.

### With test-driven-development skill
Every code-producing task follows TDD: write failing test → verify failure → write minimal code → verify pass → commit. The test spec generated by this skill provides the test names and acceptance criteria for TDD.

### With writing-plans skill
Include test specifications as part of the plan document structure. Each task's "Done when:" should reference specific test names from the spec. Test line estimates feed directly into the plan's phase estimates.

---

## Test Spec Checklist

Before finalizing a test specification, verify:

- [ ] Every design principle has one or more test cases
- [ ] Every execution flow step has success + failure variants
- [ ] Every lifecycle transition has a describe block
- [ ] All tests are classified by size (small/medium/large) AND scope (unit/integration/E2E)
- [ ] Test pyramid is approximately 80/15/5 (unit/integration/E2E)
- [ ] Negative assertions are included (not just "does X" but "does NOT do Y")
- [ ] Edge cases and error conditions are covered
- [ ] Feature flag states are tested (enabled + disabled)
- [ ] Deletion lists are included when simplifying features
- [ ] Phase plan includes test line estimates alongside implementation estimates
- [ ] Hermeticity invariants are specified (blank credentials, isolated FS, deterministic runtime)
- [ ] Baseline comparison is defined for AI-generated tests
- [ ] Given/When/Then structure is used throughout
- [ ] Test names derive from principles/flows, not implementation details
- [ ] No hallucinated assertions — every assertion traces to a spec principle or flow step

---

## Sources

- **Hermes Agent** — design principles as test axioms, execution flow as test paths, explicit test deletion lists, hermetic testing, phased test development
- **Oh My OpenAgent** — Given/When/Then grammar enforcement, factory pattern = testable, lifecycle diagrams → describe blocks, auto-regenerated specs, complexity taxonomy
- **Anthropic Skills** — expectations schema with grading, baseline comparison mandatory, non-discriminating test detection, iterative refinement, programmatic grading
- **OpenAI Codex** — SKILL.md + AGENTS.md, sandbox-aware test execution, Docker/devbox isolation
- **Aider** — exercism exercises (tests ARE spec), benchmark runner, tests as irreversible specification
- **Google Testing Philosophy** — test sizes (small/medium/large), Beyoncé Rule, 80/15/5 pyramid, size ≠ scope
- **BDD/SBE/ATDD** — Given/When/Then, Example Mapping, ubiquitous language, specs as single source of truth
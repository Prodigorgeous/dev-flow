---
name: dev-flow
description: ALWAYS use this skill. It governs your entire workflow from startup to delivery. When the user asks to build, create, implement, add, develop, make, or work on any feature, function, module, or requirement, execute the dev-flow pipeline: multi-persona product team discussion → PRD, tech lead decomposition → task breakdown, developer implementation → code, QA testing → test report, review board → merge/rework decision. Also triggers on: "I want to...", "Can you build...", "Add a feature...", "Create a...", "Develop...", "Implement...", "Make a...", "新功能", "做一个", "开发", "实现", "添加一个".
---

# Dev-Flow — Six-Phase Development Pipeline

You are governed by the dev-flow pipeline. Every new feature or requirement from the user MUST pass through the six-phase workflow below. Each phase produces a structured deliverable and **pauses for user confirmation** before proceeding to the next phase.

## Task Classification

At the start of each user request, classify the task:

- **New Feature / Requirement**: "build a login system", "add dark mode", "create an API endpoint", "implement user profiles" → Execute all 6 phases
- **Bug Fix**: "fix the login bug", "the navbar is broken" → Execute a lightweight 3-phase flow (Dev → QA → Review), skip Product and Tech Lead phases
- **Question / Conversation**: "how does JWT work?", "explain this code" → Skip the pipeline, respond normally
- **Trivial Edit**: "fix this typo", "add a comment" → Execute a single-phase fast track (just Dev, with self-review)

If uncertain about classification, ask the user: "Is this a new feature, a bug fix, or a quick tweak?"

---

## Phase 1: Product Team Discussion → PRD

**Role**: Product Management Team (multi-persona)
**Deliverable**: Product Requirements Document (PRD)
**Exit Gate**: User approves the PRD

### Step 1.1: Assemble the Product Team

Read the product team configuration from `~/.claude/dev-flow.local.md`. If not configured, use the default five-role team:

| Role | Label | Perspective |
|------|-------|-------------|
| `pragmatic` | 务实派 | Minimize complexity and cost. Ship the simplest thing that works. Respect deadlines and existing constraints. |
| `user-advocate` | 用户派 | Champion user experience above all. Users hate friction. Every extra step loses 20% of users. Make it intuitive. |
| `innovator` | 创新派 | Push boundaries. What's the differentiator? What would make this truly impressive? Think 2 years ahead. |
| `business` | 商业派 | Focus on market value, conversion rates, retention, and business metrics. Does this move the needle? |
| `tech-aware` | 技术派 | Evaluate technical feasibility, system compatibility, scalability, security, and architectural fit. |

### Step 1.2: Conduct the Discussion

For the user's requirement, role-play **each persona in sequence**, presenting their unique perspective on:

1. **Scope**: What should this feature include (and NOT include)?
2. **Priorities**: What matters most — speed? UX? scalability? cost?
3. **Risks**: What could go wrong from this persona's viewpoint?
4. **Success criteria**: How does this persona define success?

Each persona speaks in first person, in character. Keep each contribution to 3-5 sentences. Mark clearly:

```
### 👔 务实派 PM
> ...
### 💙 用户派 PM
> ...
### 🚀 创新派 PM
> ...
### 📊 商业派 PM
> ...
### ⚙️ 技术派 PM
> ...
```

### Step 1.3: Converge and Produce PRD

After all personas have spoken, **synthesize** their views into a unified PRD. Disagreements are resolved by stating the trade-off explicitly and picking a recommendation with rationale.

**PRD Format**:

```markdown
## PRD: [Feature Name]

### 1. Background & Goals
[Why this feature, what problem it solves]

### 2. Scope
**In scope**: ...
**Out of scope**: ...
**Future consideration**: ...

### 3. Functional Specification
[Detailed feature behavior, user flows, edge cases]

### 4. Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
...

### 5. Open Questions
- [Any unresolved product decisions]

### 6. Persona Votes
[Summary of each persona's stance and final recommendation]
```

### Step 1.4: Pause for User Confirmation

Present the PRD and ask:

> **以上是产品团队的 PRD，请审阅。**
> - 回复 **"通过"** 或 **"OK"** 进入 Phase 2（研发拆解）
> - 回复 **"修改 XXX"** 指定要调整的部分
> - 回复 **"重新讨论"** 让产品团队重新评估

**Do not proceed to Phase 2 until the user confirms.**

---

## Phase 2: Tech Lead Decomposition → Task Breakdown

**Role**: Technical Lead / Architect
**Deliverable**: Task Breakdown Document
**Exit Gate**: User approves the task breakdown

### Step 2.1: Analyze the PRD

Review the approved PRD from Phase 1. Identify:
- Technical domains involved (frontend, backend, database, API, etc.)
- Dependencies between components
- Existing code/modules that can be reused
- New code/modules that must be created

### Step 2.2: Produce Task Breakdown

Decompose the PRD into concrete, actionable development tasks. Each task must be:
- **Atomic**: One clear deliverable
- **Ordered**: With explicit dependencies
- **Estimable**: Rough complexity (S/M/L)
- **Verifiable**: Clear completion criteria

**Task Breakdown Format**:

```markdown
## Task Breakdown: [Feature Name]

### Architecture Overview
[Brief technical approach, key design decisions, component diagram description]

### Dependencies
- Reuses: [existing modules]
- New: [new modules to create]
- External: [APIs, libraries, services]

### Task List

#### T1: [Task Name] (S/M/L)
- **Files**: `path/to/file.ts` (new/modify)
- **Description**: [What to build]
- **Depends on**: [Task IDs or "none"]
- **Completion criteria**: [How to verify this task is done]
- **Risks**: [What could go wrong]

#### T2: [Task Name] (S/M/L)
...

### Execution Order
T1 → T2 → T3 (parallel) → T4 → T5
                T3' ─────────┘

### Estimated Total Complexity
[Summary of effort and risk]
```

### Step 2.3: Pause for User Confirmation

Present the task breakdown and ask:

> **以上是研发团队的任务拆解，请审阅。**
> - 回复 **"通过"** 或 **"OK"** 进入 Phase 3（开发编码）
> - 回复 **"修改 T3"** 指定要调整的任务
> - 回复 **"加一个任务"** 补充遗漏

**Do not proceed to Phase 3 until the user confirms.**

---

## Phase 3: Developer Implementation → Code

**Role**: Development Team
**Deliverable**: Code changes + self-check results
**Exit Gate**: User approves the implementation

### Step 3.1: Execute Tasks

Implement tasks in the order specified by Phase 2's execution plan. For each task:

1. Read the target files (understand existing code before touching it)
2. Implement the change
3. Self-check: does the code match existing style? Are changes surgical (only what's needed)?

### Step 3.2: Run Pipeline Self-Checks

Before presenting results, run the internal quality checks configured in `~/.claude/dev-flow.local.md`:

- **Pattern research**: Verify code matches existing project conventions
- **Style matching**: Verify the "reviewer test" — could a reviewer tell which lines you wrote?
- **Surgical changes**: Every changed line must be justified by the task
- **Consistency**: No debug leftovers, no commented-out blocks without explanation

### Step 3.3: Produce Implementation Summary

```markdown
## Implementation Summary: [Feature Name]

### Changes Made
| Task | File | Action | Lines |
|------|------|--------|-------|
| T1   | ...  | create | +45   |
| T2   | ...  | modify | +12/-3 |
...

### Self-Check Results
- [x] Pattern research — conventions matched
- [x] Style matching — reviewer test passed
- [x] Surgical changes — no scope creep
- [x] Consistency — no debug artifacts
- [ ] Tests — [project has no test suite / all tests pass / 1 pre-existing failure unrelated]

### Known Limitations
[Anything the developer is aware of but couldn't address in scope]
```

### Step 3.4: Pause for User Confirmation

Present the implementation summary and ask:

> **以上是开发团队的实现结果，请审阅。**
> - 回复 **"通过"** 或 **"OK"** 进入 Phase 4（测试）
> - 回复 **"修改 XX"** 指定要调整的部分
> - 回复 **"看代码"** 展示具体变更

**Do not proceed to Phase 4 until the user confirms.**

---

## Phase 4: QA Testing → Test Report

**Role**: Quality Assurance Team
**Deliverable**: Test Report
**Exit Gate**: User approves the test results

### Step 4.1: Design Test Cases

Based on the PRD (Phase 1) acceptance criteria and the implementation (Phase 3), design test cases covering:

1. **Happy path**: The feature works as expected under normal conditions
2. **Edge cases**: Boundary values, empty inputs, special characters, concurrent use
3. **Error states**: Network failure, invalid input, missing data, authentication failure
4. **Integration**: Does the new code break existing functionality?

### Step 4.2: Execute Tests

1. Run the project's existing test suite to catch regressions
2. Execute acceptance tests against the PRD criteria
3. Manually verify any behavior that can't be automated

### Step 4.3: Produce Test Report

```markdown
## Test Report: [Feature Name]

### Test Environment
- Project: [name]
- Test suite: [framework, if any]
- Manual verification: [yes/no]

### Test Results

#### Acceptance Criteria Verification
| # | Criterion (from PRD) | Status | Notes |
|---|---------------------|--------|-------|
| 1 | ...                 | ✅/❌   | ...   |

#### Regression Check
- Existing tests: [all pass / N failures: ...]
- Integration impact: [none / affected areas listed]

#### Edge Case Coverage
| Case | Input | Expected | Actual | Status |
|------|-------|----------|--------|--------|
| ...  | ...   | ...      | ...    | ✅/❌   |

### Overall Verdict
- **PASS** — All acceptance criteria met, no regressions, edge cases handled
- **PASS WITH NOTES** — Feature works but [notes]
- **FAIL** — [what failed, what needs fixing]

### Defects Found
[If any: defect description, severity, reproduction steps]
```

### Step 4.4: Pause for User Confirmation

Present the test report and ask:

> **以上是测试团队的测试报告，请审阅。**
> - 回复 **"通过"** 或 **"OK"** 进入 Phase 5（评审）
> - 回复 **"重新测试 X"** 指定要重新验证的部分
> - 如果测试未通过：回复 **"修"** 返回 Phase 3，回复 **"接受风险"** 继续

**Do not proceed to Phase 5 until the user confirms.**

---

## Phase 5: Review Board → Decision

**Role**: Review Board (independent from the dev team)
**Deliverable**: Review Decision
**Exit Gate**: User approves the review decision

### Step 5.1: Conduct Review

Review the entire pipeline output holistically:

1. **Requirement alignment**: Does the implementation match the PRD?
2. **Code quality**: Is the code maintainable, readable, and consistent?
3. **Test adequacy**: Do the tests provide sufficient confidence?
4. **Risk assessment**: What is the residual risk? Are there open questions?
5. **Architectural impact**: Does this change align with the project's architecture?

### Step 5.2: Produce Review Decision

```markdown
## Review Decision: [Feature Name]

### Review Summary
[2-3 sentence overall assessment]

### Dimension Scores
| Dimension | Score | Notes |
|-----------|-------|-------|
| Requirement Alignment | 1-5 | ... |
| Code Quality | 1-5 | ... |
| Test Coverage | 1-5 | ... |
| Risk Level | Low/Med/High | ... |
| Architectural Fit | 1-5 | ... |

### Decision
- **✅ APPROVED — Merge** — [rationale]
- **⚠️ CONDITIONALLY APPROVED** — Merge after [conditions]
- **❌ RETURNED** — Return to [Phase X] for [reason]

### Action Items
- [ ] [Action 1]
- [ ] [Action 2]
```

### Step 5.3: Pause for User Confirmation

Present the review decision and ask:

> **以上是评审结果，请审阅。**
> - 回复 **"通过"** 或 **"OK"** 进入 Phase 6（合入）
> - 回复 **"返工"** 并指定返回哪个阶段

**Do not proceed to Phase 6 until the user confirms.**

---

## Phase 6: Merge or Rework

Based on the Phase 5 decision:

### If APPROVED → Merge

1. If the project is a git repository, stage and commit the changes with a structured commit message:
   ```
   feat: [Feature Name]

   [Brief description from PRD]

   Tasks: T1, T2, T3...
   Reviewed-by: Dev-Flow Pipeline
   ```

2. Present the final summary:
   ```markdown
   ## ✅ Pipeline Complete: [Feature Name]
   
   | Phase | Status | Deliverable |
   |-------|--------|-------------|
   | 1. Product | ✅ | PRD approved |
   | 2. Tech Lead | ✅ | N tasks defined |
   | 3. Developer | ✅ | Implemented |
   | 4. QA | ✅ | All tests pass |
   | 5. Review | ✅ | Approved |
   | 6. Merge | ✅ | Committed |
   ```

### If CONDITIONALLY APPROVED → Apply Conditions

Complete the specified conditions, then re-run Phase 4 (QA) and Phase 5 (Review) on the updated implementation.

### If RETURNED → Go Back

Return to the specified phase and re-execute from there. The return path can be:
- **Return to Phase 1 (Product)**: Requirements need clarification
- **Return to Phase 3 (Developer)**: Implementation needs changes
- **Return to Phase 4 (QA)**: Needs additional testing

---

## Pipeline Configuration

All configuration is read from `~/.claude/dev-flow.local.md` at startup. If absent, defaults apply. Configuration is managed via the `/dev-flow` command.

**Default Product Team** (5 roles):
- `pragmatic` (务实派) — minimize complexity, ship fast
- `user-advocate` (用户派) — user experience above all
- `innovator` (创新派) — push boundaries, differentiate
- `business` (商业派) — market value, business metrics
- `tech-aware` (技术派) — feasibility, compatibility, security

**Auto-load skills**: Additional skills to invoke at startup (empty by default).

For full configuration reference, see `references/config-guide.md`.

## Delivery Rules

1. **Every phase pauses for user confirmation.** Never skip a pause unless the user explicitly says "skip confirmation" or "auto-approve all phases."
2. **Every phase produces a structured deliverable.** Use the formats specified above.
3. **The pipeline is visible.** Always show which phase you're in and what's coming next.
4. **Respect user time.** If the user says "just do it, skip the pipeline," respect that. The pipeline is a tool, not a jail.

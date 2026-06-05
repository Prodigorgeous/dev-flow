# Default Pipeline Specification — Full Rationale

This document explains the design rationale behind each phase and step of the set-workflow six-phase pipeline. Loaded on demand when deeper context is needed.

---

## Why a Role-Playing Pipeline?

Traditional development workflows separate concerns across teams: product defines what to build, engineering figures out how, development builds it, QA validates it, and a review board decides if it ships. Each team brings a distinct perspective that catches issues the others miss.

Set-Workflow replicates this structure by having Claude role-play each team in sequence. The role-playing constraint forces the model to:
1. **Consider the problem from multiple angles** before writing code — product fit, technical feasibility, user experience, business value
2. **Produce intermediate artifacts** (PRD, task breakdown, test report) that the user can inspect and correct before code is written
3. **Separate concerns** — the developer doesn't decide what to build (product does), and the reviewer isn't the same person who wrote the code

---

## Phase 1: Product Team — Multi-Persona Discussion

**Why multiple personas?** A single PM has blind spots. The pragmatic PM ships fast but misses user experience. The user advocate builds great UX but overengineers. The innovator thinks big picture but ignores deadlines. By running all perspectives and then converging, the PRD captures trade-offs explicitly rather than implicitly.

**Why converge before proceeding?** The personas will disagree. That's the point. The convergence step forces a synthesis — a single recommendation that acknowledges the disagreements and explains the trade-off. This is more valuable than a unanimous PRD because it surfaces the tensions the implementation will need to navigate.

**Why pause for user confirmation?** The user knows things Claude cannot: team politics, upcoming roadmap items, budget constraints, user research. The Phase 1 pause is the user's opportunity to inject this context before engineering effort is spent.

---

## Phase 2: Tech Lead — Task Decomposition

**Why decompose before coding?** Jumping straight to code on a multi-file feature leads to:
- Missing dependencies discovered mid-implementation
- Wrong file structure requiring refactoring
- Parallelizable work done sequentially (wasting user time)
- Tasks that are too large to complete in one session

A 5-minute decomposition saves 30-minute rewrites.

**Why explicit dependencies?** Tasks with unclear dependencies lead to "chicken-and-egg" blocks during implementation. Listing dependencies upfront forces the tech lead to think through the execution order.

**Why complexity labels (S/M/L)?** Size labels help the user gauge total effort and decide scope cuts. If every task is "L," the feature may need descoping before implementation starts.

---

## Phase 3: Developer — Implementation

**Why read before writing?** The most common source of style violations and pattern breaks is insufficient context. Reading target files and similar files before coding eliminates entire categories of review feedback.

**Why self-checks?** The developer is the first line of quality. Self-checks (pattern research, style matching, surgical changes, consistency) catch issues that would otherwise be found downstream by QA or review — when fixes are more expensive.

**Why a structured summary?** The summary gives QA (Phase 4) and Review (Phase 5) a clear map of what changed and why. Without it, those phases spend time rediscovering the implementation rather than evaluating it.

---

## Phase 4: QA — Testing

**Why a separate QA phase?** The developer who writes code is the worst person to test it. They know the "correct" path so thoroughly that edge cases become invisible. A separate QA phase — even role-played by the same Claude — brings fresh eyes.

**Why acceptance criteria verification first?** If the feature doesn't meet the PRD's acceptance criteria, finding subtle edge-case bugs is wasted effort. Acceptance-first testing catches requirement misalignment early.

**Why explicit edge case coverage?** Edge cases that are listed and checked are handled. Edge cases that are never thought about become production bugs. The coverage table makes the invisible visible.

**Why a verdict, not just results?** A raw test log requires the reader to interpret. A verdict ("PASS", "PASS WITH NOTES", "FAIL") provides immediate, actionable information. The user can read the verdict in 2 seconds and dive deeper if needed.

---

## Phase 5: Review Board — Decision

**Why a review board separate from the developer?** Code review by the author is self-review — useful but insufficient. A separate review phase, evaluating the entire pipeline output (PRD → tasks → code → tests), catches issues that span phases. Maybe the code is good but doesn't match the PRD. Maybe the tests are thorough but for the wrong acceptance criteria.

**Why dimension scores?** A single "approved/rejected" decision hides nuance. Dimension scores show exactly where the feature is strong and weak, enabling targeted rework rather than blanket rejection.

---

## Phase 6: Merge or Rework

**Why a structured commit message?** The pipeline produces rich context (PRD, tasks, test report, review). The commit message distills the essential information: what was built, what tasks were involved, and that it passed the pipeline. This is far more useful than "feat: add login" when someone reads the git log 6 months later.

**Why return paths are explicit?** "Return to Phase 1" is a different decision than "Return to Phase 3." Phase 1 means the requirements are wrong — the entire downstream needs re-execution. Phase 3 means the implementation has a bug — the PRD and task breakdown are still valid. Explicit return paths prevent re-doing work that didn't need re-doing.

---

## Task Classification Logic

The pipeline distinguishes four task types to avoid over-process:

| Type | Trigger Phrases | Pipeline |
|------|----------------|----------|
| New Feature | "build", "create", "add", "implement", "make a..." | Full 6 phases |
| Bug Fix | "fix", "bug", "broken", "not working" | 3 phases (Dev → QA → Review) |
| Trivial Edit | "typo", "add a comment", "rename" | Fast track (Dev only, self-review) |
| Conversation | "explain", "what is", "how does" | No pipeline |

**Why classify?** A 6-phase pipeline is heavy. Applying it to a typo fix is wasteful and annoying. Classification ensures the process matches the task.

---

## User Confirmation: Why Every Phase?

The user chose "every phase confirmation" because:
1. **Early correction is cheap.** Wrong PRD assumptions caught in Phase 1 save redoing Phases 2-6.
2. **User has irreplaceable context.** Claude doesn't know the team's roadmap, the user's preferences, or unwritten conventions.
3. **Pipeline visibility builds trust.** Seeing each phase's output before the next phase starts helps the user understand how decisions are made.

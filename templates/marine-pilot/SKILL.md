---
name: marine-pilot
description: Full autonomous pipeline — plan, execute, QA, validate using marine team
---

# /marine-pilot

> "Marines, move out." — Full autonomous execution from idea to verified code using team marine.

## Usage

```bash
/marine-pilot <task description>
/marine-pilot <task> --skip-qa       # Skip QA cycling
/marine-pilot <task> --skip-validate # Skip multi-perspective review
```

## When to Use

- User says "marine-pilot", "marines go", "จัดการให้หมด", "autopilot"
- Multi-phase task: planning + coding + testing + validation
- User wants hands-off execution

## Do NOT Use When

- Quick fix or single-file change — delegate directly
- Exploring options — use `/deep-dive`
- Just verifying a change — use `/verify`

## Pipeline

```
Phase 0: Understand → Phase 0.5: Code Recon → Phase 1: Plan → Phase 2: Execute → Phase 3: QA → Phase 4: Validate
```

### Phase 0 — Understand

1. Analyze the request
2. If vague, ask ONE clarifying question (max)
3. Identify scope: files, systems, risks

### Phase 0.5 — Code Recon (MANDATORY before planning)

**Goal**: Understand what the code ACTUALLY does, not what the user assumes it does.

Spawn **marine-research** (Explore agent) to read the relevant code before forming any plan:

1. Read all files related to the reported issue/feature area
2. Trace the full data flow: input → processing → output
3. Identify the REAL state vs the user's assumption
4. Surface surprises: hidden bugs, existing implementations, wrong mental models

**Output required before Phase 1:**
- What the code actually does (not what the requirement says)
- Gap between user assumption and reality
- Files + line numbers that will need to change
- Any blockers or risks found in the code itself

**Why this matters**: User requirements describe symptoms, not root causes.
Reading code first prevents planning for the wrong problem.

Example — "Open Positions missing field" → Code recon found field EXISTS but wrong isResolved threshold hides positions. Planning without recon would have added a new field instead of fixing the real bug.

### Phase 1 — Plan

1. **Marine Research** (Haiku): Investigate current code, constraints, dependencies
2. **Marine Prime** creates implementation plan with:
   - Tasks broken down with clear file paths
   - Dependencies mapped
   - Agent assignments per task
   - Risk flags
3. Present plan → **wait for human approval**

### Phase 2 — Execute

Model routing (token-efficient):
- **Haiku**: Simple lookups, file reads, type fixes, doc updates
- **Sonnet**: Standard implementation, component creation, API changes
- **Opus**: Complex refactoring, architecture changes, security-sensitive code

Execute tasks in parallel when independent:
```
// Good: parallel independent tasks
[marine-frontend, Haiku] → "Add loading spinner to button component"
[marine-backend, Sonnet] → "Create /api/users endpoint"
[marine-devops, Haiku] → "Add health check to docker-compose"
```

### Phase 3 — QA Cycling

Repeat up to 5 cycles:
1. Build (`bun run build` / `npm run build`)
2. Typecheck (`tsc --noEmit`)
3. Lint
4. Tests
5. If failures → fix → repeat

**Stop conditions:**
- All checks pass → proceed to Phase 4
- Same error 3 times in a row → stop, report fundamental issue
- Cycle 5 reached → report remaining issues

### Phase 4 — Validate

Multi-perspective review (can run in parallel):
1. **Marine Security**: Vulnerability scan (OWASP Top 10 check)
2. **Marine Research**: Architecture consistency review
3. **Build verification**: Fresh build + test run

All reviewers must approve. On rejection → fix → re-validate (max 3 rounds).

### Phase 5 — Report

```markdown
## Mission Complete

### What was built
[summary of changes]

### Files modified
| File | Change | Agent |
|------|--------|-------|
| ... | ... | ... |

### Verification
- Build: PASS
- Tests: PASS (X/Y)
- Security review: PASS
- Architecture review: PASS

### QA Cycles
X cycles, Y issues fixed

### Notes
[anything notable, caveats, follow-ups]
```

## State Persistence

Progress tracked in `.marine/` (gitignored):
```
.marine/
├── pilot-state.json    # Current phase, task statuses
├── plan.md             # Approved plan
└── qa-log.md           # QA cycle history
```

On cancel: progress is preserved. Run `/marine-pilot` again to resume.

## Rules

- NEVER skip Phase 1 (plan) unless `--plan-exists` flag
- ALWAYS wait for human approval before Phase 2
- NEVER merge or push — human decides
- If blocked, report immediately — don't silently retry
- If unsure about scope, ask before assuming

## Escalation

- **Blocked**: Report blocker, wait for human input
- **Ambiguous requirements**: Ask one question, don't guess
- **QA stuck**: Same error 3x → stop and report
- **Validation fails 3x**: Escalate to human with summary

## Examples

### Good
```
/marine-pilot add user authentication with JWT to the API
→ Phase 0: scope identified (backend + frontend + tests)
→ Phase 1: plan with 7 tasks across 3 agents
→ Phase 2: parallel execution
→ Phase 3: QA cycling
→ Phase 4: security review + build check
→ Report delivered
```

### Bad
```
/marine-pilot fix typo in README
```
Why bad: Too small. Use direct edit instead.

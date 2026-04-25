---
name: marine-orchestrator
description: Multi-agent orchestration — coordinate marine team across repos, worktrees, sprints, and critical decision gates
---

# /marine-orchestrator

> "All units, move in formation." — Coordinate marine team for multi-agent, multi-repo missions.

## Usage

```bash
/marine-orchestrator <task>                    # Coordinate agents for a task
/marine-orchestrator sprint                    # Sprint planning from open issues
/marine-orchestrator standup                   # Daily standup format
/marine-orchestrator deploy <repo>             # Deploy with critical decision gate
/marine-orchestrator worktree <feature> <repos> # Create parallel worktrees
```

## When to Use

- Task needs 2+ marine agents coordinated
- Feature spans multiple repos (bot-polymarket + haocomm-ai-oracle)
- Sprint planning or task breakdown needed
- Critical deploy that needs human gate
- Worktree isolation for risky experiment

## Do NOT Use When

- Single-agent task → use `/marine-pilot` instead
- Quick one-file fix → delegate directly
- Exploration only → use marine-research

---

## Pipeline

```
Phase 0: Parse intent
Phase 1: Route to agents (sequential or parallel)
Phase 2: Human gate (if critical)
Phase 3: Execute + track state
Phase 4: Report
```

---

### Phase 0 — Parse Intent

Identify:
1. Task type (feature / sprint / deploy / worktree / ceremony)
2. Repos involved (1 or many)
3. Risk level (need human gate?)
4. Which marine agents needed

---

### Phase 1 — Agent Routing

**Routing table:**

| Task Type | Agent |
|-----------|-------|
| API / backend logic | marine-backend |
| Dashboard / UI | marine-frontend |
| Prompt / AI provider | marine-prompt-engineer |
| Trade analysis, statistics | marine-analyst |
| SSH / deploy / prod | marine-devops |
| Code exploration | marine-research |
| Architecture decisions | marine-ai |
| Blog / content / docs | marine-content |
| Security review | marine-security |
| Position / PnL logic | marine-trading |
| Full pipeline | marine-pilot |

**Parallel when independent:**
```
marine-backend  ─┐
marine-frontend ─┼→ merge when both done
marine-devops   ─┘
```

**Sequential when dependent:**
```
marine-research → marine-backend → marine-analyst
```

---

### Phase 2 — Critical Decision Gate (MANDATORY)

Required before:
- Production deploy
- positions.json reset / trade close
- Strategy/threshold changes (confidence, edge, stop-loss)
- Deleting files on prod
- Any action affecting live capital

**Format — always show this before proceeding:**

```
⚠️ CRITICAL DECISION: [Action]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What:  [specific action]
Why:   [reasoning]
Risk:  [impact to prod/funds/positions]

Approve? (y/n):
```

Never skip this gate. Never auto-approve.

---

### Phase 3 — Execute + Track

Track state in conversation or `.marine/orchestrator-state.json`:

```json
{
  "mission": "take-profit-50",
  "github_issue": "#10",
  "phase": "execute",
  "agents": {
    "marine-trading": "in_progress",
    "marine-analyst": "pending"
  },
  "worktrees": ["bot-polymarket/feature-take-profit"]
}
```

---

### Phase 4 — Report

```markdown
## Mission Complete

### Agents deployed
| Agent | Task | Status |
|-------|------|--------|
| marine-trading | implement take-profit | ✅ done |
| marine-analyst | verify no threshold change | ✅ done |
| marine-devops | deploy to prod | ✅ done |

### Files changed
[list]

### Human gates triggered
[list of gates + decisions]

### Next
[follow-up items]
```

---

## Ceremony Templates

### `/marine-orchestrator standup`

```
## Daily Standup — [DATE]

✅ Done:
- [completed tasks]

🔄 Today:
- [planned tasks]

🚧 Blockers:
- [issues needing help]

📊 Bot status:
- PnL: $X | Win rate: X% | Open: X positions
```

### `/marine-orchestrator sprint`

```
1. List open GitHub issues
2. Estimate each: S (< 2h) / M (2-4h) / L (4h+)
3. Assign marine agent per issue
4. Define done criteria
5. Set human checkpoint schedule
```

---

## Worktree Workflow

```bash
# Create isolated worktrees for parallel dev
git worktree add ../bot-polymarket-feature-X feature/X
git worktree add ../haocomm-feature-X feature/X

# Assign agents to each
# After merge — cleanup
git worktree remove ../bot-polymarket-feature-X
git worktree remove ../haocomm-feature-X
```

Use worktrees when:
- Breaking change needs testing before prod
- Two repos need simultaneous changes
- Hotfix while main dev continues

---

## Quality Gates (non-negotiable)

- Phase 0.5 Code Recon ALWAYS before planning (inherited from marine-pilot)
- marine-analyst: n < 50 resolved trades = "cannot conclude" — no threshold changes
- marine-prompt-engineer: before/after action distribution required for prompt changes
- marine-devops: prod deploy requires human gate above
- Human decides on capital/strategy — orchestrator never auto-decides

---

## RTK — Token Optimization (MANDATORY)

Always prefix bash: `rtk read`, `rtk git status`, `rtk ls`, `rtk git log`
Skip rtk only for: `npm install`, `node -e`, `npx`, `ssh`, `gh`, `git push`

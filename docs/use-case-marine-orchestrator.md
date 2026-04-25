# Use Case: GSD + marine-orchestrator

marine-orchestrator คือ coordinator สำหรับ multi-agent, multi-repo missions
ใช้เมื่องาน span หลาย repos หรือต้องการหลาย marine agents ทำงานพร้อมกัน

---

## เมื่อใช้ marine-orchestrator vs marine-pilot

| Situation | ใช้ |
|-----------|-----|
| Task เดียว, 1 repo | `/marine-pilot` |
| Feature span 2+ repos | `/marine-orchestrator worktree` |
| Sprint planning | `/marine-orchestrator sprint` |
| Production deploy | `/marine-orchestrator deploy` |
| 2+ marine agents parallel | `/marine-orchestrator` |
| Ceremonies (standup, retro) | `/marine-orchestrator standup` |

---

## Pattern 1: Multi-Repo Feature

**ตัวอย่าง**: feature ใหม่ที่ต้องแก้ทั้ง bot-polymarket + haocomm-ai-oracle

```bash
/marine-orchestrator worktree "take-profit" "bot-polymarket haocomm-ai-oracle"
```

Orchestrator:
1. สร้าง worktree แยกสำหรับแต่ละ repo
2. Route: `marine-backend` → bot-polymarket logic, `marine-analyst` → haocomm analytics
3. ทั้ง 2 agents ทำงานพร้อมกัน (parallel)
4. Critical gate ก่อน merge กลับ main

---

## Pattern 2: Sprint Planning จาก GSD Roadmap

```bash
/marine-orchestrator sprint
```

Orchestrator อ่าน GSD roadmap → แตก GitHub issues → assign marine agents → estimate

Output:
```markdown
## Sprint — 2026-04-25

| Issue | Estimate | Agent |
|-------|----------|-------|
| #12: take-profit | M (2-4h) | marine-trading |
| #13: blog posts | S (<2h) | marine-content |
| #14: node cleanup | S (<2h) | marine-devops |

Gate: ทุก issue ผ่าน GSD verify ก่อน merge
```

---

## Pattern 3: Production Deploy Gate

```bash
/marine-orchestrator deploy bot-polymarket
```

Orchestrator แสดง critical decision gate:

```
⚠️ CRITICAL DECISION: Deploy bot-polymarket to production
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What:  git pull + pm2 restart on 204.168.182.194
Why:   take-profit feature verified + merged to main
Risk:  live trading bot — affects open positions

Approve? (y/n):
```

Human approve → marine-devops deploy → verify → report

---

## Pattern 4: GSD Phase + Orchestrator Multi-Agent

**ตัวอย่าง**: phase ที่ต้องการ frontend + backend + analytics พร้อมกัน

```bash
/gsd-plan-phase 3    # GSD research + plan (3 PLAN.md files)

# แทน gsd-execute-phase, ใช้ orchestrator:
/marine-orchestrator execute gsd phase 3
```

Orchestrator:
```
marine-backend    ─── PLAN-3-1.md (API endpoints)
marine-frontend   ─── PLAN-3-2.md (dashboard UI)    → parallel
marine-analyst    ─── PLAN-3-3.md (analytics logic)
                           ↓
                    integration merge
                           ↓
                    marine-security review
```

---

## ตัวอย่างเต็ม: Multi-Agent Feature บน bot-polymarket

### Context
- ต้องการ: lottery take-profit + analytics dashboard + deploy
- Scope: bot-polymarket code + haocomm UI

### Commands

```bash
# 1. Plan ด้วย GSD
/gsd-discuss-phase 4        # lock: take-profit 50%, chart.js dashboard
/gsd-plan-phase 4           # research + 3 PLAN.md files

# 2. Orchestrate execution
/marine-orchestrator
# → route marine-trading (take-profit), marine-frontend (dashboard)
# → parallel execution
# → marine-analyst verify no threshold changes

# 3. Critical gate ก่อน deploy
/marine-orchestrator deploy bot-polymarket
# → human approve → marine-devops deploy

# 4. Verify + Ship
/gsd-verify-work 4
/gsd-ship 4
```

---

## Orchestrator Ceremonies กับ GSD State

### Daily Standup

```bash
/marine-orchestrator standup
```

ดึง GSD progress อัตโนมัติ:
```
## Daily Standup — 2026-04-25

✅ Done:
- Phase 3: lottery analyzer fix (GSD verified)
- marine-orchestrator docs site live

🔄 Today:
- Phase 4: take-profit implementation

🚧 Blockers:
- n=22 resolved trades (ต้องรอ 50+ ก่อน strategy analysis)

📊 Bot: PnL -$60.51 | Win rate 36% | Open: 2 positions
```

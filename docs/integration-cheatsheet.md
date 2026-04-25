# Integration Cheat Sheet

GSD + Marine Team — คำสั่งที่ใช้บ่อยในหน้าเดียว

---

## Decision Tree: เลือก Tool ไหน?

```
มีทุน/production เกี่ยวข้อง?
  YES → /marine-orchestrator (critical gate)
  NO ↓

หลาย repos?
  YES → /marine-orchestrator worktree
  NO ↓

ต้องการ 2+ marine agents พร้อมกัน?
  YES → /marine-orchestrator
  NO ↓

ต้องการ QA cycling + security review?
  YES → /marine-pilot
  NO ↓

งานเล็ก 1-2 files?
  YES → /gsd-fast หรือ edit โดยตรง
  NO → /gsd-execute-phase N
```

---

## GSD Commands ที่ใช้ทุกวัน

```bash
/gsd-progress              # อยู่ที่ไหน, ต่อไปทำอะไร
/gsd-next                  # auto-run next step
/gsd-fast "งานเล็ก"        # ข้าม planning
/gsd-note "idea"           # capture ทันที
/gsd-add-todo "task"       # เพิ่ม todo
/gsd-pause-work            # handoff ก่อนหยุด
/gsd-resume-work           # resume จาก handoff
```

---

## Marine Team Commands ที่ใช้ทุกวัน

```bash
/recap                     # orient: handoff + git + tracks
/rrr                       # retrospective
/forward                   # handoff + plan ก่อน session จบ
/marine-pilot <task>       # task เดียว autonomous
/marine-orchestrator sprint # sprint planning
/marine-orchestrator standup # daily standup
```

---

## Full Phase Workflow

```bash
# 1. Lock preferences
/gsd-discuss-phase N

# 2. Research + plan
/gsd-plan-phase N

# 3a. Execute (simple)
/gsd-execute-phase N

# 3b. Execute (complex/QA needed)
/marine-pilot implement gsd phase N

# 3c. Execute (multi-agent)
/marine-orchestrator execute gsd phase N

# 4. Verify
/gsd-verify-work N

# 5. Ship
/gsd-ship N
```

---

## สร้าง Project ใหม่

```bash
/gsd-new-project
# ตอบคำถาม → PROJECT.md + ROADMAP.md

# จากนั้น loop:
/gsd-discuss-phase 1
/gsd-plan-phase 1
/marine-pilot implement gsd phase 1
/gsd-verify-work 1
/gsd-ship 1
```

---

## Bot-Polymarket Specific

```bash
# วิเคราะห์ overnight
/marine-pilot วิเคราะห์ bot status

# Check lottery
pm2 logs bot-polymarket --lines 50 | grep LOTTERY

# Check positions
cat /home/deploy-app/bot-polymarket/data/positions.json | python3 -c "
import json,sys
data=json.load(sys.stdin)
for p in data.get('positions',[]):
    if p.get('status') in ['open','live']: print(p.get('question','?')[:60], p.get('status'))
"

# Deploy
/marine-orchestrator deploy bot-polymarket
```

---

## Marine Agent Routing

| งาน | Agent |
|-----|-------|
| API / backend logic | marine-backend |
| Dashboard / UI | marine-frontend |
| Prompt / AI provider | marine-prompt-engineer |
| Trade analysis, PnL | marine-analyst |
| SSH / deploy / prod | marine-devops |
| Code exploration | marine-research |
| Architecture | marine-ai |
| Blog / docs | marine-content |
| Security review | marine-security |
| Position / lottery logic | marine-trading |

---

## RTK (Token Saver) — สำหรับ marine agents

```bash
# ใน marine agent bash commands ใช้ rtk prefix เสมอ:
rtk git status
rtk git log --oneline -5
rtk ls -la src/
rtk read src/agents/lotteryAnalyzer.js

# ยกเว้น (ไม่ต้อง rtk):
npm install
npx ...
ssh ...
gh ...
git push
```

---

## Principled Analysis Rules

> **marine-analyst enforce เสมอ:**
> - n < 50 resolved trades = "ยังสรุป strategy ไม่ได้"
> - ห้ามเปลี่ยน confidence threshold, stop-loss, edge ก่อนถึง n=50
> - ดูแค่ตัวเลขจริง — ไม่ใช่ตัวเลขที่อยากเห็น

---

## Critical Decision Gate Template

ต้องแสดงก่อน actions เหล่านี้เสมอ:
- Production deploy
- positions.json reset
- Strategy threshold changes
- Delete files on prod
- Actions affecting live capital

```
⚠️ CRITICAL DECISION: [Action]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What:  [specific action]
Why:   [reasoning]
Risk:  [impact to prod/funds/positions]

Approve? (y/n):
```

**Never auto-approve. Human decides.**

# Full Lifecycle Example

ตัวอย่าง project ตั้งแต่ต้นจนจบ — bot-polymarket style

---

## Project: Bot Trading บน Polymarket

### ความต้องการ
- Bot วิเคราะห์ตลาด prediction → วางคำสั่งซื้อขาย
- Lottery analyzer: หาตลาดที่ underpriced
- Position manager: track + close positions

---

## Phase 0: เริ่ม Project ใหม่

```bash
/gsd-new-project
```

GSD ถาม:
- Project name และ description
- Tech stack preferences
- Success criteria

Output: `PROJECT.md`, `ROADMAP.md` พร้อม 6 phases

---

## Phase 1: Core Infrastructure

```bash
# Discussion
/gsd-discuss-phase 1
# → ตอบ: Node.js, ESM modules, dotenv, ไม่ใช้ TypeScript ตอนแรก

# Plan
/gsd-plan-phase 1
# → research: Polymarket CLOB API, order book structure
# → PLAN-1-1.md: config + providers
# → PLAN-1-2.md: basic order placement

# Execute via marine-pilot (ต้องการ QA cycling)
/marine-pilot implement gsd phase 1
```

**marine-pilot pipeline:**
```
Code Recon:  อ่าน API docs, existing patterns
Plan:        approve PLAN.md → wait
Execute:     AnthropicProvider, GeminiProvider, config.js
QA:          3 cycles → lint + integration tests
Validate:    security (no API key leak in logs)
Report:      4 files, 2 QA cycles, PASS
```

---

## Phase 2: Market Analysis Agent

```bash
/gsd-discuss-phase 2
# → Gemini Flash สำหรับ research (cost-efficient)
# → Claude Sonnet สำหรับ final analysis
# → thinkingBudget: 0 สำหรับ JSON format step

/gsd-plan-phase 2

/marine-pilot implement lotteryAnalyzer with 2-step Gemini pattern
```

**Key insight จาก code recon:**

marine-pilot Phase 0.5 พบว่า:
- Gemini 2.5 Flash มี thinking overhead แม้ thinkingBudget:0
- maxTokens ต้องอย่างน้อย 2048 สำหรับ JSON tasks
- extractJson() parser ต้องรอง handle truncated JSON

---

## Phase 3: Position Manager

```bash
/gsd-discuss-phase 3
/gsd-plan-phase 3

# Multi-agent: trading logic + analytics
/marine-orchestrator execute gsd phase 3
```

Orchestrator route:
```
marine-trading  → lotteryPositions.js, positionManager.js
marine-analyst  → verify: n<50 = cannot conclude strategy changes
```

**Critical gate:**
```
⚠️ CRITICAL: Reset positions.json to test take-profit
Risk: clears all position history
→ Human approve
```

---

## Phase 4: Take-Profit (เพิ่มทีหลัง)

หลัง n=22 resolved trades → ยังไม่ถึง 50 → defer strategy changes

```bash
/gsd-add-backlog "take-profit at 50% — wait n>=50"
/gsd-plant-seed "take-profit" --trigger "resolved trades >= 50"
```

เมื่อ n ≥ 50:

```bash
/gsd-insert-phase 4   # แทรก phase ใหม่
/gsd-discuss-phase 4  # lock: 50% threshold, use sizeMatched field
/gsd-plan-phase 4
/marine-pilot implement take-profit logic
```

---

## Phase 5: Deploy + Monitor

```bash
# Deploy via orchestrator (critical gate)
/marine-orchestrator deploy bot-polymarket

# Monitor
/marine-pilot วิเคราะห์ bot status หลัง deploy 24h
```

marine-pilot วิเคราะห์:
- `decisions.jsonl` → fill rate, action distribution
- `positions.json` → win rate, PnL
- `trades.jsonl` → executed fills only

**Principled analysis rule:**
> n < 50 resolved trades = "ยังไม่สามารถสรุป strategy ได้" — marine-analyst enforce เสมอ

---

## Phase 6: Ship

```bash
/gsd-verify-work 5   # UAT: ทดสอบ golden path + edge cases
/gsd-ship 5          # PR พร้อม rich description
/gsd-complete-milestone "v1.0"
```

---

## Lifecycle Summary

```
/gsd-new-project
    ↓
[phase loop:]
/gsd-discuss-phase N  →  /gsd-plan-phase N  →  /marine-pilot หรือ /marine-orchestrator
    ↓
/gsd-verify-work N  →  /gsd-ship N
    ↓
/gsd-complete-milestone
    ↓
/gsd-new-milestone "v2.0"
```

---

## Tips จาก Production Experience

1. **marine-analyst เสมอ** — ก่อน strategy change ใดๆ ต้องผ่าน marine-analyst
2. **pm2 logs มี history** — เช็ค timestamp ก่อนสรุปว่า error active
3. **lotteryAnalyzer: maxTokens ≥ 2048** — Gemini thinking overhead กิน budget
4. **fill rate ต่ำ ≠ bug** — CLOB order ที่ไม่ match คือ market dynamics ปกติ
5. **n < 50 = no strategy conclusions** — รอก่อน อย่าเปลี่ยน threshold

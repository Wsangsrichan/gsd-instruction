# Getting Started — Full Flow

เริ่มจากเครื่องเปล่า ใช้ทุกเครื่องมือร่วมกันจนได้ project ชั้น production

---

## ก่อนเริ่ม: สิ่งที่ต้องติดตั้ง

```bash
# 1. Claude Code (CLI)
npm install -g @anthropic-ai/claude-code

# 2. GSD — meta-prompting + workflow
npx get-shit-done-cc@latest

# 3. RTK — token optimization (ประหยัด 60-90%)
# ติดตั้งจาก source, ดู RTK docs

# 4. SocratiCode — codebase intelligence
# ติดตั้งผ่าน Claude Code marketplace (Plugins → socraticode)
# ต้องมี Docker + Qdrant + Ollama รันอยู่
codebase_health  # verify ✅✅✅

# 5. Matt Pocock Skills — TDD, diagnose, architecture
git clone https://github.com/mattpocock/skills.git /tmp/mattpocock-skills
cp -r /tmp/mattpocock-skills/skills/engineering/* ~/.claude/skills/
cp -r /tmp/mattpocock-skills/skills/productivity/* ~/.claude/skills/

# 6. Marine Team — autonomous multi-agent
# ติดตั้งจาก gsd-instruction templates
# ดู: installation-marine.md

# 7. ตรวจสอบ
/gsd-help          # 60+ commands
/marine-pilot      # marine pipeline
/tdd               # TDD skill
codebase_health    # SocratiCode
```

---

## Stack ภาพรวม

```
┌─────────────────────────────────────────────┐
│              คุณ (Human)                     │
├─────────────────────────────────────────────┤
│  Marine Team (orchestrator, pilot, agents)  │  ← execute
├─────────────────────────────────────────────┤
│  GSD (workflow, phases, plans, verification)│  ← structure
├──────────┬──────────┬───────────────────────┤
│ MattPocock│ Socrati │ RTK                   │  ← discipline
│ TDD/Diag │ Code    │ Token Save            │
├──────────┴──────────┴───────────────────────┤
│            Claude Code (CLI)                 │  ← runtime
└─────────────────────────────────────────────┘
```

---

## Flow 1: เริ่ม Project ใหม่ (Greenfield)

### Step 1: สร้าง project + roadmap

```bash
/gsd-new-project
```

GSD ถาม requirements → สร้าง `PROJECT.md` + `ROADMAP.md`

### Step 2: ลงมือ phase แรก

```bash
/gsd-discuss-phase 1          # lock preferences (auth, DB, patterns)
/gsd-plan-phase 1             # research + atomic plans
```

### Step 3: Execute ด้วย TDD (Matt Pocock)

```bash
/tdd
```

TDD vertical slices:
```
test1 → impl1 → test2 → impl2 → test3 → impl3
```

ทีละ test ทีละ implementation ไม่เขียน test ทั้งหมดก่อน

### Step 4: หรือ execute ด้วย marine-pilot (ถ้าต้อง QA + security)

```bash
/marine-pilot implement gsd phase 1
```

marine-pilot pipeline:
```
Code Recon → Plan → Execute → QA (5 cycles) → Validate → Report
```

### Step 5: Verify + Ship

```bash
/gsd-verify-work 1            # UAT + automated checks
/gsd-ship 1                   # PR
```

### Loop

```bash
/gsd-next                     # auto-detect → run next step
# หรือ
/gsd-discuss-phase 2
/gsd-plan-phase 2
/marine-pilot implement gsd phase 2
/gsd-verify-work 2
/gsd-ship 2
```

---

## Flow 2: เข้า Project ที่มีอยู่แล้ว (Brownfield)

### Step 1: เข้าใจ codebase ด้วย SocratiCode

```bash
codebase_index projectPath="/path/to/project"
codebase_status  # poll จน 100%

codebase_graph_stats           # ภาพรวม: hub files, orphans, circular deps
codebase_graph_visualize mode="interactive"  # แผนที่ interactive
codebase_flow entrypoint="main" depth=4      # execution flow
```

**ตัวอย่างผลลัพธ์ (bot-polymarket):**
```
Total files: 57
Most connected: config.js (27), index.js (21)
Circular deps: 0
Orphans: dashboard/*, scripts/*

Flow: main → runCycle → runArbCycle
      main → runLotteryCycle
      main → startApiServer
```

5 นาที → เข้าใจ architecture

### Step 2: สร้าง GSD structure

```bash
/gsd-map-codebase              # วิเคราะห์ codebase เดิม
/gsd-new-project               # สร้าง ROADMAP จาก codebase ที่มี
```

### Step 3: เริ่มทำงาน

```bash
/gsd-discuss-phase 1
/gsd-plan-phase 1
/gsd-execute-phase 1
/gsd-verify-work 1
/gsd-ship 1
```

---

## Flow 3: Debug ปัญหา (Diagnose + SocratiCode)

### Step 1: ค้นหา code ที่เกี่ยวข้อง

```bash
codebase_search query="heuristic confidence threshold"
codebase_graph_query filePath="src/agents/trader.js"
codebase_flow entrypoint="main" depth=4
```

### Step 2: เริ่ม diagnose

```bash
/diagnose
```

Diagnose 6 phases:
```
1. Build feedback loop (failing test / curl / script)
2. Reproduce the bug
3. Hypothesise (3-5 ranked hypotheses)
4. Instrument (one variable at a time)
5. Fix + regression test
6. Cleanup + post-mortem
```

### Step 3: ถ้าเป็น architecture issue

```bash
/improve-codebase-architecture   # หา deepening opportunities
/gsd-add-phase                   # เพิ่ม refactor phase
/marine-pilot implement refactor
```

---

## Flow 4: Feature ใหม่บน Project ที่มีอยู่

### Step 1: สร้าง PRD (Matt Pocock)

```bash
/to-prd
```

Output: PRD พร้อม user stories, implementation decisions, testing decisions

### Step 2: Grill plan (stress-test)

```bash
/grill-me                        # สัมภาษณ์ plan
# หรือ
/grill-with-docs                 # + อัปเดต CONTEXT.md + ADRs
```

### Step 3: แตกเป็น issues

```bash
/to-issues
```

แตก PRD → vertical slices (แต่ละ slice demoable ได้เอง)

### Step 4: Triage issues

```bash
/triage                         # ดู issues ที่ต้อง attention
/triage #42                     # ตรวจสอบ issue เดียว
```

State machine: needs-triage → ready-for-agent → executing

### Step 5: Execute ด้วย GSD

```bash
/gsd-discuss-phase N
/gsd-plan-phase N
/marine-pilot implement gsd phase N
/gsd-verify-work N
/gsd-ship N
```

---

## Flow 5: ทุกเครื่องมือร่วมกัน (Full Stack)

ตัวอย่าง: เพิ่ม "auto take-profit" ใน bot-polymarket

```
Step 1: เข้าใจ codebase (SocratiCode)
  codebase_search "position management"
  codebase_flow entrypoint="runCycle" depth=3
  codebase_impact target="src/positions.js"
  → เห็นว่า positions.js เป็น hub ถ้าแก้ตรงนี้ กระทบ 5 files

Step 2: สร้าง PRD (Matt Pocock)
  /to-prd → user stories, implementation decisions

Step 3: Stress-test plan (Matt Pocock)
  /grill-me → "50% threshold หรือ configurable?"
  /grill-with-docs → อัปเดต CONTEXT.md ด้วยคำว่า "take-profit"

Step 4: แตกเป็น issues (Matt Pocock)
  /to-issues → 4 vertical slices

Step 5: Triage (Matt Pocock)
  /triage → ทุก slice = ready-for-agent

Step 6: เพิ่ม phase ใน GSD
  /gsd-add-phase → "Auto Take-Profit"
  /gsd-discuss-phase N → lock: 50% default, configurable via env

Step 7: Plan ด้วย GSD
  /gsd-plan-phase N → research + atomic plans

Step 8: Execute ด้วย marine-pilot + TDD
  /marine-pilot implement gsd phase N
  → Phase 0.5: code recon (SocratiCode: codebase_search "stop loss")
  → Phase 2: execute ด้วย /tdd vertical slices
  → Phase 3: QA cycling (3 cycles)
  → Phase 4: validate (security + architecture)

Step 9: Verify + Ship
  /gsd-verify-work N → UAT: test take-profit triggers correctly
  /gsd-ship N → PR

Step 10: Retrospective
  /rrr → บันทึก lesson learned
```

---

## Decision Tree: ใช้อะไรเมื่อไร

```
เข้า project ใหม่?
  YES → /gsd-new-project
  NO ↓

เข้า codebase ที่มีอยู่?
  YES → codebase_index → codebase_graph_stats → /gsd-map-codebase
  NO ↓

เริ่ม feature ใหม่?
  YES → /to-prd → /grill-me → /to-issues → /triage
  NO ↓

Debug ปัญหา?
  YES → codebase_search → /diagnose
  NO ↓

ปรับ architecture?
  YES → /improve-codebase-architecture → /gsd-add-phase
  NO ↓

ต้องการ QA + security?
  YES → /marine-pilot
  NO ↓

งานเล็ก?
  YES → /gsd-fast "งานเล็ก"
  NO → /gsd-execute-phase N
```

---

## Quick Reference Card

| ทำอะไร | Command |
|--------|---------|
| เริ่ม project | `/gsd-new-project` |
| เข้าใจ codebase | `codebase_index` → `codebase_graph_stats` |
| หาโค้ด | `codebase_search "query"` |
| ดู flow | `codebase_flow entrypoint="main"` |
| ตรวจ impact | `codebase_impact target="file"` |
| สร้าง PRD | `/to-prd` |
| Stress-test plan | `/grill-me` |
| แตกเป็น issues | `/to-issues` |
| จัด issues | `/triage` |
| Lock preferences | `/gsd-discuss-phase N` |
| Research + plan | `/gsd-plan-phase N` |
| Execute (simple) | `/gsd-execute-phase N` |
| Execute (TDD) | `/tdd` |
| Execute (full pipeline) | `/marine-pilot` |
| Execute (multi-agent) | `/marine-orchestrator` |
| Debug | `/diagnose` |
| Architecture | `/improve-codebase-architecture` |
| Verify | `/gsd-verify-work N` |
| Ship | `/gsd-ship N` |
| ดู progress | `/gsd-progress` |
| Next step | `/gsd-next` |
| Retrospective | `/rrr` |
| จำ state | `/gsd-pause-work` → `/gsd-resume-work` |

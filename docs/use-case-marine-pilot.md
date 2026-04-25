# Use Case: GSD + marine-pilot

marine-pilot คือ full autonomous execution pipeline — Plan → Execute → QA → Validate
GSD คือ foundation ที่ให้ structure: phase artifacts, research, verification gates

ทั้งสองทำงานร่วมกันได้ใน 2 patterns:

---

## Pattern 1: GSD เป็น Foundation, marine-pilot เป็น Executor

**เหมาะกับ**: งาน feature ที่มี scope ชัดเจน, ต้องการ QA cycling + security validation

```
/gsd-discuss-phase 1        # lock preferences
/gsd-plan-phase 1           # research + PLAN.md
/marine-pilot execute gsd phase 1   # marine executes the GSD plan
```

marine-pilot อ่าน PLAN.md จาก GSD → execute ตาม tasks → QA cycle → validate → report

---

## Pattern 2: marine-pilot สร้าง feature ใหม่บน GSD project

**เหมาะกับ**: เพิ่ม feature กลาง project ที่มี GSD structure อยู่แล้ว

### ตัวอย่าง: เพิ่ม take-profit logic ใน bot-polymarket

```bash
# Step 1: สร้าง phase ใหม่ใน GSD
/gsd-insert-phase 3    # แทรกระหว่าง phase ที่มีอยู่

# Step 2: ให้ marine-pilot implement
/marine-pilot implement take-profit 50% for lotteryPositions
```

marine-pilot pipeline:
```
Phase 0:   Understand scope
Phase 0.5: Code Recon → อ่าน lotteryPositions.js, positionManager.js
Phase 1:   Plan → present to user → wait approval
Phase 2:   Execute → implement
Phase 3:   QA → build + typecheck + tests (up to 5 cycles)
Phase 4:   Validate → security + architecture review
Report:    files changed, QA cycles, test results
```

---

## ตัวอย่างเต็ม: New Feature บน bot-polymarket

### Context
- bot-polymarket มี GSD structure อยู่แล้ว (`.planning/`)
- ต้องการเพิ่ม auto-close position เมื่อกำไร 50%

### Step-by-step

**1. Add phase to GSD roadmap:**
```bash
/gsd-add-phase
# → ตอบคำถาม: feature name, requirements, acceptance criteria
```

**2. Discuss preferences:**
```bash
/gsd-discuss-phase N
# ตอบ: trigger at 50% profit, use existing positionManager, no new deps
```

**3. Research + Plan:**
```bash
/gsd-plan-phase N
# GSD spawns research agents → reads existing code patterns
# Output: RESEARCH.md + PLAN.md files
```

**4. marine-pilot Execute:**
```bash
/marine-pilot implement phase N from GSD plan
```

marine-pilot:
- อ่าน `phase-N-PLAN.md`
- Phase 0.5: code recon บน `src/lotteryPositions.js`, `src/positionManager.js`
- Implement take-profit logic
- QA: unit tests + integration
- Security: no capital-at-risk issues
- Report พร้อม diff

**5. Verify + Ship:**
```bash
/gsd-verify-work N   # UAT
/gsd-ship N          # PR
```

---

## marine-pilot Commands สำหรับ GSD

```bash
# Analyze current GSD state
/marine-pilot วิเคราะห์ gsd progress และแนะนำ next steps

# Execute specific phase
/marine-pilot implement gsd phase 2 ตาม plan ที่มี

# Fix failing QA
/marine-pilot แก้ failing tests ใน phase 1

# Quick feature (ข้าม full GSD flow)
/marine-pilot --skip-qa เพิ่ม health check endpoint
```

---

## เมื่อใช้ marine-pilot vs gsd-execute-phase

| Situation | ใช้ |
|-----------|-----|
| งาน feature ปกติ ตาม plan | `/gsd-execute-phase N` |
| ต้องการ QA cycling อัตโนมัติ | `/marine-pilot` |
| ต้องการ security validation | `/marine-pilot` |
| งานซับซ้อน หลาย files | `/marine-pilot` |
| งานเล็ก 1-2 files | `/gsd-fast` หรือ direct edit |

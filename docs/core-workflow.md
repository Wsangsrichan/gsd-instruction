# Core Workflow

GSD workflow เป็น phase-based, research-informed, และ verification-gated

```
Discuss → Plan → Execute → Verify → Ship → Repeat
```

แต่ละ phase เป็น iteration อิสระ — fresh context, atomic commits, verification gate

---

## Phase Lifecycle

### 1. Discuss (ล็อค preferences)

```bash
/gsd-discuss-phase 1
```

บอก AI ว่าต้องการอะไรก่อน plan — ป้องกัน AI เดาผิดทาง

ตัวอย่าง questions:
- ใช้ auth แบบไหน? JWT หรือ session?
- database ORM หรือ raw query?
- error handling pattern อย่างไร?

Output: `phase-1-CONTEXT.md` — lock preferences ถาวร

---

### 2. Plan (research + แตก tasks)

```bash
/gsd-plan-phase 1
```

GSD spawn parallel research agents → สร้าง atomic execution plans

Output:
- `phase-1-RESEARCH.md` — patterns, dependencies, gotchas
- `phase-1-1-PLAN.md`, `phase-1-2-PLAN.md`, ... — atomic tasks (2-3 tasks each)

> แต่ละ PLAN.md เล็กพอให้ AI อ่านได้ใน fresh context window เดียว

---

### 3. Execute (parallel waves)

```bash
/gsd-execute-phase 1
```

GSD รัน plans เป็น **waves**:
- Wave 1: independent tasks → ขนานกัน
- Wave 2: tasks ที่ depends on Wave 1 → หลังจาก Wave 1 เสร็จ
- Wave 3: integration tasks → ท้ายสุด

แต่ละ task:
- fresh context (ไม่มี rot)
- isolated worktree (ถ้า risky)
- atomic commit เมื่อเสร็จ

---

### 4. Verify (UAT + automated checks)

```bash
/gsd-verify-work 1
```

- Automated: typecheck, lint, tests
- Manual UAT: user ทดสอบ golden path + edge cases
- Output: `phase-1-VERIFICATION.md`

---

### 5. Ship (PR)

```bash
/gsd-ship 1
```

สร้าง PR branch จาก verified work พร้อม rich description

---

## State Machine

```
unplanned → discussed → planned → executing → executed → verified → shipped
```

ดู state ปัจจุบัน:

```bash
/gsd-progress      # where am I, what's next
/gsd-next          # auto-detect และรัน step ต่อไป
```

---

## Phase Artifacts

| File | สร้างตอนไหน | ใช้ทำอะไร |
|------|------------|----------|
| `CONTEXT.md` | /gsd-discuss-phase | lock preferences |
| `RESEARCH.md` | /gsd-plan-phase | patterns + gotchas |
| `N-PLAN.md` | /gsd-plan-phase | atomic execution task |
| `N-SUMMARY.md` | /gsd-execute-phase | สรุปสิ่งที่ build |
| `VERIFICATION.md` | /gsd-verify-work | automated check results |
| `UAT.md` | /gsd-verify-work | manual test results |

---

## Quick Mode (ข้าม planning)

สำหรับงานเล็ก ไม่ต้อง full workflow:

```bash
/gsd-fast "แก้ bug ใน lotteryAnalyzer line 99"
/gsd-quick "เพิ่ม endpoint GET /api/positions"
```

---

## Phase Granularity

```bash
/gsd-settings    # ตั้ง granularity
```

| Setting | Phases | เหมาะกับ |
|---------|--------|---------|
| `coarse` | 3-5 | MVP เร็ว |
| `standard` | 5-8 | ปกติ |
| `fine` | 8-12 | parallel-heavy |

---

## Model Profiles

```bash
/gsd-set-profile quality     # claude-opus (complex planning)
/gsd-set-profile balanced    # claude-sonnet (default)
/gsd-set-profile budget      # claude-haiku (simple tasks)
```

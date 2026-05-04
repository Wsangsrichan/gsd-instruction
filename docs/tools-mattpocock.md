# Matt Pocock Skills — Integration

Discipline-first skills สำหรับ TDD, debug, architecture จาก [mattpocock/skills](https://github.com/mattpocock/skills)

---

## ติดตั้ง

```bash
git clone https://github.com/mattpocock/skills.git /tmp/mattpocock-skills
cp -r /tmp/mattpocock-skills/skills/engineering/* ~/.claude/skills/
cp -r /tmp/mattpocock-skills/skills/productivity/* ~/.claude/skills/
```

---

## Skills สำคัญที่สุด

| Skill | Command | ทำอะไร |
|-------|---------|--------|
| **TDD** | `/tdd` | Vertical slices: ทีละ test→impl (ไม่ใช่ทีละชั้น) |
| **Diagnose** | `/diagnose` | 6-phase debug: feedback loop → reproduce → hypothesise → fix |
| **Zoom Out** | `/zoom-out` | ดูภาพรวม codebase เมื่อเข้า area ใหม่ |
| **Triage** | `/triage` | State machine จัด issues (needs-triage → ready-for-agent) |
| **To PRD** | `/to-prd` | สร้าง PRD จาก conversation context |
| **To Issues** | `/to-issues` | แตก plan → vertical slices (แต่ละ slice demoable) |
| **Grill with Docs** | `/grill-with-docs` | สัมภาษณ์ plan + อัปเดต CONTEXT.md/ADRs |
| **Improve Architecture** | `/improve-codebase-architecture` | หา deepening opportunities (shallow→deep modules) |

---

## ใช้ร่วมกับ GSD

### /tdd + GSD Execute

```bash
/gsd-plan-phase 1                    # GSD สร้าง PLAN.md
/tdd                                  # ทำตาม PLAN ด้วย TDD vertical slices
```

GSD plan แตก tasks → TDD execute ทีละ task ด้วย red-green-refactor

### /diagnose + GSD Debug

```bash
/gsd-debug "heuristic analyzer ให้ edge ผิด"
# หรือใช้ diagnose ตรง:
/diagnose
```

### /improve-codebase-architecture + GSD Plan

```bash
/improve-codebase-architecture       # หา deepening opportunities
/gsd-add-phase                       # เพิ่ม refactor phase จาก findings
```

### /to-prd + GSD New Project

```bash
/to-prd                               # สร้าง PRD จาก requirements
/gsd-new-project                      # สร้าง project structure จาก PRD
```

---

## Key Concepts

### Vertical Slices (ไม่ใช่ Horizontal)

```
WRONG:  test1,test2,test3 → impl1,impl2,impl3
RIGHT:  test1→impl1 → test2→impl2 → test3→impl3
```

ทุกอย่างใน Matt Pocock เป็น vertical — TDD, issues, planning

### Deep Modules

- **Deep** = interface เล็ก, ซ่อน complexity → ดี
- **Shallow** = interface ซับซ้อนเท่า code → pass-through
- **Deletion test**: ลบ module ออก ถ้า complexity หาย = ไม่มีคุณค่า

### Feedback Loop First

`/diagnose` บังคับสร้าง repro ก่อนคิดสาเหตุ — ไม่มี loop ไม่มี debug

---

## เปรียบเทียบ: Matt Pocock vs Marine

| | Matt Pocock | Marine |
|--|-------------|--------|
| สไตล์ | Discipline + philosophy | Military + autonomous |
| TDD | ละเอียด (vertical slices, deep modules) | มีแต่คร่าวกว่า |
| Debug | `/diagnose` 6 phases แบบมีระบบ | `/gsd-debug` |
| Multi-agent | ไม่มี | marine-orchestrator |
| Architecture | `/improve-codebase-architecture` | marine-ai |
| ใช้ร่วมกัน | ได้ — TDD เข้ากับ marine-pilot | ได้ — architecture เข้ากับ marine-ai |

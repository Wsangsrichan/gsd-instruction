# ติดตั้ง GSD

## Quick Install (แนะนำ)

```bash
npx get-shit-done-cc@latest
```

ถามว่าต้องการ runtime ไหนและติดตั้ง global หรือ local

---

## Non-Interactive Install

### Claude Code (ที่ marine team ใช้)

```bash
# Global (ใช้ทุก project)
npx get-shit-done-cc --claude --global

# Local (เฉพาะ project นี้)
npx get-shit-done-cc --claude --local
```

### Runtimes อื่น

```bash
npx get-shit-done-cc --opencode --global
npx get-shit-done-cc --gemini --global
npx get-shit-done-cc --codex --global
npx get-shit-done-cc --cursor --global
npx get-shit-done-cc --windsurf --global

# ทุก runtime พร้อมกัน
npx get-shit-done-cc --all --global
```

---

## อัพเดท GSD

```bash
npx get-shit-done-cc@latest
```

GSD พัฒนาเร็ว — อัพเดทบ่อยๆ เพื่อ features ใหม่

---

## ตรวจสอบการติดตั้ง

```bash
/gsd-help    # ใน Claude Code — แสดง commands ทั้งหมด
```

ถ้าเห็น command list = ติดตั้งสำเร็จ

---

## Dev Install (จาก source)

```bash
git clone https://github.com/gsd-build/get-shit-done.git
cd get-shit-done
npm run build:hooks
node bin/install.js --claude --local
```

---

## Requirements

- Node.js 22+
- Claude Code (หรือ runtime อื่นที่รองรับ)
- Git

---

## โครงสร้างหลังติดตั้ง

GSD สร้าง commands และ agents ใน:

```
~/.claude/           # global install
  commands/gsd-*     # 60+ slash commands
  agents/            # specialized agent definitions
  hooks/             # SessionStart, PostToolUse hooks

.claude/             # local install (same structure)
```

และ project ที่ใช้ GSD จะมี:

```
.planning/
  STATE.md           # project state
  ROADMAP.md         # phase breakdown
  phase-N/           # artifacts per phase
    CONTEXT.md
    RESEARCH.md
    PLAN.md
    SUMMARY.md
    VERIFICATION.md
```

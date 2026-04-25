# GSD คืออะไร?

**GSD (Get Shit Done)** คือ meta-prompting และ context engineering system สำหรับ Claude Code และ AI assistants อื่นๆ

แทนที่จะ vibecode (บอก AI → หวังให้ถูก → ปรับแก้ไม่จบ), GSD ให้ AI มีทุกอย่างที่ต้องการ:
- requirements ที่ชัดเจน
- patterns ที่ research มาแล้ว
- plan ที่แตกออกเป็น tasks เล็กๆ
- verification gates ทุก phase

ผลลัพธ์: code ที่สม่ำเสมอ ไม่มี context rot ไม่ต้องแก้ซ้ำ

---

## ปัญหาที่ GSD แก้

### Context Rot

Context window เต็ม → AI ลืมสิ่งที่คุยไปก่อน → ผลงานคุณภาพตก

GSD แก้โดย: แต่ละ task รัน fresh context → ไม่มี rot

### Vibe Coding

```
"ทำ auth ให้หน่อย" → AI เดา → ผิดทาง → แก้ 10 รอบ
```

GSD แก้โดย: discuss → lock preferences ก่อน → plan → execute ตามแผน

### Lost Context ข้าม session

ครั้งหน้าต้องอธิบาย context ใหม่ทั้งหมด

GSD แก้โดย: `PROJECT.md`, `RESEARCH.md`, `PLAN.md` เก็บ context ถาวร

---

## GSD vs วิธีปกติ

| | วิธีปกติ | GSD |
|--|---------|-----|
| เริ่มงาน | บอก AI ตรงๆ | discuss → plan → execute |
| Context | เต็มและ rot | fresh per task |
| Commit | bulk | atomic per task |
| ทดสอบ | ทำเองทั้งหมด | verification gates อัตโนมัติ |
| ข้าม session | อธิบายใหม่ | PROJECT.md จำให้ |
| Multi-agent | ยาก | wave execution built-in |

---

## GSD ทำงานกับอะไร

Claude Code, OpenCode, Gemini CLI, Kilo, Codex, Copilot, Cursor, Windsurf และอีก 5+ runtimes

---

## Marine Team + GSD

ใน Haocomm marine team, GSD ทำงานร่วมกัน 2 ระดับ:

| Tool | Role |
|------|------|
| `/gsd-plan-phase` | Research + plan phase (GSD core) |
| `/gsd-execute-phase` | Execute parallel waves (GSD core) |
| `/marine-pilot` | Single task: plan → execute → QA → validate |
| `/marine-orchestrator` | Multi-agent, multi-repo, ceremonies |

GSD = foundation. Marine team = executor on top of GSD structure.

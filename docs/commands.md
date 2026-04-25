# Command Reference

GSD มี 60+ commands แบ่งตามหมวด

---

## Workflow หลัก

| Command | ทำอะไร |
|---------|--------|
| `/gsd-new-project` | เริ่ม project ใหม่: questions → research → requirements → roadmap |
| `/gsd-discuss-phase [N]` | lock implementation preferences ก่อน plan |
| `/gsd-plan-phase [N]` | research + plan + verify phase N |
| `/gsd-execute-phase <N>` | run plans ใน parallel waves พร้อม atomic commits |
| `/gsd-verify-work [N]` | UAT + automated checks |
| `/gsd-ship [N]` | สร้าง PR จาก verified work |
| `/gsd-next` | auto-detect state → run next step |
| `/gsd-quick [flags]` | ad-hoc task ด้วย GSD guarantees |
| `/gsd-fast <text>` | inline trivial task ข้าม planning |

---

## Phase Management

| Command | ทำอะไร |
|---------|--------|
| `/gsd-add-phase` | เพิ่ม phase ท้าย roadmap |
| `/gsd-insert-phase [N]` | แทรก urgent work ระหว่าง phases |
| `/gsd-remove-phase [N]` | ลบ phase อนาคต + renumber |
| `/gsd-list-phase-assumptions [N]` | ดูว่า AI วางแผน approach อะไรก่อน plan |
| `/gsd-plan-milestone-gaps` | สร้าง phases ปิด verification gaps |

---

## Milestone & Workspace

| Command | ทำอะไร |
|---------|--------|
| `/gsd-audit-milestone` | verify milestone achieved definition of done |
| `/gsd-complete-milestone` | archive milestone + tag release |
| `/gsd-new-milestone [name]` | เริ่ม version ต่อไป |
| `/gsd-new-workspace [--name]` | สร้าง isolated workspace |
| `/gsd-list-workspaces` | แสดง workspaces ทั้งหมด |
| `/gsd-workstreams` | parallel workstreams: list, create, switch, complete |

---

## Session & State

| Command | ทำอะไร |
|---------|--------|
| `/gsd-progress` | อยู่ที่ไหน? ต่อไปทำอะไร? |
| `/gsd-pause-work` | สร้าง handoff เมื่อหยุดกลางทาง |
| `/gsd-resume-work` | restore จาก session ล่าสุด |
| `/gsd-session-report` | summary session + outcomes |

---

## Spiking & Sketching

| Command | ทำอะไร |
|---------|--------|
| `/gsd-spike [idea]` | throwaway experiments ทดสอบ feasibility |
| `/gsd-sketch [idea]` | interactive HTML mockup สำรวจ design |
| `/gsd-spike-wrap-up` | package spike findings เป็น skill |
| `/gsd-sketch-wrap-up` | package sketch decisions เป็น skill |

---

## Brownfield (Existing Codebases)

| Command | ทำอะไร |
|---------|--------|
| `/gsd-map-codebase [area]` | วิเคราะห์ codebase เดิมก่อน new-project |
| `/gsd-ingest-docs [dir]` | scan ADRs, PRDs, SPECs → bootstrap `.planning/` |

---

## Code Quality

| Command | ทำอะไร |
|---------|--------|
| `/gsd-review [N]` | cross-AI peer review |
| `/gsd-code-review [N] [--depth]` | code review ก่อน UAT |
| `/gsd-code-review-fix [N]` | auto-fix code review issues |
| `/gsd-secure-phase [N]` | security enforcement + threat model |
| `/gsd-ui-review [N]` | 6-pillar visual audit |
| `/gsd-pr-branch` | clean PR branch ไม่มี `.planning/` commits |
| `/gsd-audit-uat` | find phases missing UAT |

---

## Debugging

| Command | ทำอะไร |
|---------|--------|
| `/gsd-debug [desc]` | systematic debugging + persistent state |
| `/gsd-forensics [desc]` | post-mortem บน failed workflow |
| `/gsd-health [--repair]` | validate `.planning/` integrity + auto-repair |

---

## Utilities

| Command | ทำอะไร |
|---------|--------|
| `/gsd-note <text>` | capture idea ทันที |
| `/gsd-add-todo [desc]` | เพิ่ม todo |
| `/gsd-check-todos` | list pending todos |
| `/gsd-settings` | configure model profile + workflow agents |
| `/gsd-set-profile <profile>` | switch: quality / balanced / budget |
| `/gsd-stats` | project statistics |
| `/gsd-help` | แสดง commands ทั้งหมด |
| `/gsd-update` | update GSD + changelog preview |

---

## Flags ที่ใช้บ่อย

| Flag | ใช้กับ | ทำอะไร |
|------|--------|--------|
| `--full` | `/gsd-quick` | research + plan + execute |
| `--validate` | `/gsd-quick` | + verification gate |
| `--discuss` | `/gsd-quick` | + discussion phase |
| `--depth quick\|standard\|deep` | `/gsd-code-review` | ระดับ review |
| `--repair` | `/gsd-health` | auto-fix integrity issues |
| `--quick` | `/gsd-spike`, `/gsd-sketch` | faster, less thorough |

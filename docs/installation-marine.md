# ติดตั้ง Marine Team

marine-pilot และ marine-orchestrator คือ skills สำหรับ Claude Code
ติดตั้งโดย copy ไฟล์ไปยัง `~/.claude/` directory

---

## Prerequisites

- Claude Code (CLI หรือ IDE extension)
- Node.js 18+
- Git

ตรวจสอบ Claude Code:
```bash
claude --version
```

---

## โครงสร้าง ~/.claude/

```
~/.claude/
├── skills/                    # slash commands
│   ├── marine-pilot/
│   │   └── SKILL.md
│   └── marine-orchestrator/
│       └── SKILL.md
└── agents/
    └── marine/                # sub-agents
        ├── marine-backend.md
        ├── marine-frontend.md
        ├── marine-research.md
        ├── marine-trading.md
        ├── marine-devops.md
        ├── marine-security.md
        ├── marine-analyst.md
        ├── marine-content.md
        ├── marine-ai.md
        ├── marine-prompt-engineer.md
        └── marine-prime.md
```

---

## ติดตั้ง marine-pilot

```bash
mkdir -p ~/.claude/skills/marine-pilot
curl -o ~/.claude/skills/marine-pilot/SKILL.md \
  https://raw.githubusercontent.com/Wsangsrichan/gsd-instruction/main/templates/marine-pilot/SKILL.md
```

หรือ copy จาก template ด้านล่างและสร้างไฟล์เอง:

```bash
mkdir -p ~/.claude/skills/marine-pilot
nano ~/.claude/skills/marine-pilot/SKILL.md
```

ทดสอบ:
```bash
# ใน Claude Code session พิมพ์:
/marine-pilot
```

---

## ติดตั้ง marine-orchestrator

```bash
mkdir -p ~/.claude/skills/marine-orchestrator
curl -o ~/.claude/skills/marine-orchestrator/SKILL.md \
  https://raw.githubusercontent.com/Wsangsrichan/gsd-instruction/main/templates/marine-orchestrator/SKILL.md
```

ทดสอบ:
```bash
/marine-orchestrator sprint
```

---

## ติดตั้ง Marine Agents

agents คือ sub-agents ที่ marine-pilot และ marine-orchestrator ใช้ spawn

```bash
mkdir -p ~/.claude/agents/marine
# copy ทุก agent:
for agent in marine-backend marine-frontend marine-research marine-trading \
             marine-devops marine-security marine-analyst marine-content \
             marine-ai marine-prompt-engineer marine-prime; do
  curl -o ~/.claude/agents/marine/${agent}.md \
    https://raw.githubusercontent.com/Wsangsrichan/gsd-instruction/main/templates/agents/${agent}.md
done
```

---

## Marine Agent ทั้งหมด

| Agent | บทบาท | ใช้เมื่อ |
|-------|--------|---------|
| `marine-backend` | API, database, server-side logic | endpoints, auth, migrations |
| `marine-frontend` | UI components, CSS, UX | dashboard, forms, layouts |
| `marine-research` | Code exploration, doc review | code recon, tech comparison |
| `marine-trading` | Order lifecycle, PnL, exchanges | trading bot logic, risk rules |
| `marine-devops` | SSH, deploy, pm2, Docker | production deploy, CI/CD |
| `marine-security` | OWASP review, threat model | security audit, key rotation |
| `marine-analyst` | Statistics, data analysis | PnL analysis, win rate, n-threshold |
| `marine-content` | Blog posts, docs, markdown | writing, documentation |
| `marine-ai` | Architecture, AI/LLM decisions | provider selection, prompt strategy |
| `marine-prompt-engineer` | Prompt design, token optimization | AI provider switching, prompt tuning |
| `marine-prime` | Lead orchestration | complex multi-agent coordination |

---

## SKILL.md Format

skill file บอก Claude Code ว่า command ทำอะไรและ pipeline ทำงานอย่างไร

```markdown
---
name: marine-pilot
description: Full autonomous pipeline — plan, execute, QA, validate using marine team
---

# /marine-pilot

> "Marines, move out."

## Usage

```bash
/marine-pilot <task description>
/marine-pilot <task> --skip-qa
/marine-pilot <task> --skip-validate
```

## When to Use
- Multi-phase task: planning + coding + testing + validation
- User wants hands-off execution

## Pipeline

```
Phase 0: Understand
Phase 0.5: Code Recon (MANDATORY)
Phase 1: Plan → wait for human approval
Phase 2: Execute
Phase 3: QA Cycling (up to 5 cycles)
Phase 4: Validate
Phase 5: Report
```

## Rules
- NEVER skip Phase 1 (plan)
- ALWAYS wait for human approval before Phase 2
- NEVER merge or push — human decides
```

**สำคัญ**: YAML frontmatter (`---`) จำเป็นสำหรับ Claude Code ให้รู้จัก skill

---

## Agent Format

agent file คือ prompt template สำหรับ sub-agent แต่ละตัว

```markdown
# Marine Backend Engineer

Role: Builds and maintains APIs, databases, integrations, and server-side logic.

## Expertise
- API design (REST, GraphQL, WebSocket)
- Database modeling, migrations, query optimization
- Authentication and authorization
- Node.js, Python, Go

## When to Use
- Work involves server-side code, API contracts, schema changes
- Database operations or query optimization
- Authentication flows, security middleware

## Workflow
1. Read existing code and understand current patterns
2. Propose changes with clear rationale
3. Implement with proper error handling
4. Verify with tests or manual checks

## Quality Standards
- All API endpoints must have proper error responses
- Database queries must be parameterized (no SQL injection)
- Sensitive data must not be logged
- Follow existing project conventions

## Anti-Patterns to Avoid
- Don't add features beyond what was requested
- Don't skip error handling
- Don't store secrets in code

## RTK — Token Optimization (MANDATORY)

Always prefix bash commands with `rtk`:
- `rtk git status` instead of `git status`
- `rtk ls <dir>` instead of `ls <dir>`
- `rtk read <file>` instead of `cat <file>`
```

---

## สร้าง Custom Agent

ถ้าต้องการ agent เฉพาะทาง เช่น `marine-iot` หรือ `marine-ml`:

```bash
cat > ~/.claude/agents/marine/marine-ml.md << 'EOF'
# Marine ML Engineer

Role: Builds and maintains machine learning pipelines, model training, and inference.

## Expertise
- Model training, evaluation, and deployment
- Feature engineering and data preprocessing
- Python: scikit-learn, PyTorch, pandas, numpy
- MLOps: experiment tracking, model versioning

## When to Use
- Implementing ML models or pipelines
- Feature engineering
- Model evaluation and A/B testing

## Workflow
1. Understand the ML problem (classification/regression/clustering)
2. Review existing data and feature pipeline
3. Implement with reproducibility in mind (seeds, versioning)
4. Evaluate with proper train/val/test split

## Quality Standards
- Never train on test data
- Always log model metrics (loss, accuracy, F1)
- Save model artifacts with version tags
- Reproducible experiments (fixed random seeds)

## RTK — Token Optimization (MANDATORY)
Always prefix bash: rtk git status, rtk ls, rtk read
EOF
```

---

## Verify Installation

```bash
# ตรวจสอบ structure
ls ~/.claude/skills/
ls ~/.claude/agents/marine/

# ใน Claude Code:
/marine-pilot ทดสอบ installation
```

ถ้า `/marine-pilot` ทำงาน → ติดตั้งสำเร็จ

---

## หมายเหตุ: RTK Token Optimization

ทุก marine agent ใช้ `rtk` prefix สำหรับ bash commands เพื่อประหยัด token 60-90%

```bash
# ติดตั้ง rtk (ถ้ายังไม่มี)
# rtk คือ token-optimized CLI proxy สำหรับ Claude Code
```

agents จะ prefix คำสั่งอัตโนมัติ — ไม่ต้องทำเอง

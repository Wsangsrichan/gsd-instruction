# SocratiCode — Integration

Codebase intelligence MCP plugin สำหรับ Claude Code — semantic search, dependency graph, flow tracing

---

## ติดตั้ง

```bash
# 1. ติดตั้ง plugin ผ่าน Claude Code marketplace
# Settings → Plugins → search "socraticode"

# 2. ตรวจสอบ infrastructure
codebase_health
# ต้องได้: Docker ✅, Qdrant ✅, Ollama ✅

# 3. Index project
codebase_index projectPath="/path/to/project"
codebase_status  # poll จน 100%
```

---

## Tools สำคัญ

| Tool | ทำอะไร | ใช้เมื่อ |
|------|--------|----------|
| `codebase_search` | Semantic search ด้วยภาษาธรรมดา | หาโค้ดโดยไม่ต้องจำ function name |
| `codebase_graph_query` | Imports + importers ของไฟล์ | อยากรู้ใครเรียก/ใช้ไฟล์นี้ |
| `codebase_graph_stats` | ภาพรวม codebase (hub files, orphans, circular deps) | เข้า project ใหม่ |
| `codebase_graph_visualize` | Mermaid diagram หรือ interactive HTML | ดูแผนที่ codebase |
| `codebase_flow` | Trace call tree จาก function | เข้าใจ execution flow |
| `codebase_impact` | Blast radius ก่อนแก้โค้ด | Refactor อย่างปลอดภัย |
| `codebase_symbols` | List/ค้นหา functions, classes | สำรวจ API surface |
| `codebase_watch` | Auto-update index เมื่อไฟล์เปลี่ยน | Development ต่อเนื่อง |

---

## ใช้ร่วมกับ GSD

### ก่อน /gsd-new-project

```bash
# วิเคราะห์ codebase เดิมก่อนเริ่ม project
codebase_index projectPath="/path/to/existing-project"
codebase_graph_stats
codebase_graph_visualize mode="interactive"
```

### ก่อน /gsd-plan-phase

```bash
# Research patterns ที่มีอยู่แล้ว
codebase_search query="authentication middleware"
codebase_graph_query filePath="src/auth.js"
codebase_flow entrypoint="handleRequest" depth=3
```

### ก่อน /gsd-execute-phase

```bash
# ตรวจ blast radius ก่อนแก้
codebase_impact target="src/config.js"
codebase_graph_circular  # ตรวจ circular deps
```

### ก่อน /marine-pilot (Phase 0.5 Code Recon)

```bash
# Code recon ด้วย SocratiCode แทน manual file reading
codebase_search query="order placement flow"
codebase_flow entrypoint="main" depth=4
codebase_symbols file="src/trader.js"
```

SocratiCode เร็วกว่า manual reading — ค้นหา semantic ทันที

---

## ตัวอย่าง: bot-polymarket

### เข้าใจ architecture ใน 5 นาที

```bash
# Index
codebase_index projectPath="/home/deploy-app/bot-polymarket"
# → 65 files, 345 chunks

# ภาพรวม
codebase_graph_stats
# → config.js: 27 connections (hub)
# → index.js: 21 connections (entry point)
# → 0 circular deps

# Flow
codebase_flow entrypoint="main" depth=4
# → main → runCycle → runArbCycle
# → main → runLotteryCycle
# → main → startApiServer

# ค้นหา
codebase_search query="confidence threshold routing"
# → เจอ trader.js ที่ route AI vs heuristic
```

### ก่อนแก้ heuristic analyzer

```bash
codebase_graph_query filePath="src/agents/heuristicAnalyzer.js"
# imports: config.js, sportsData.js, logger.js
# imported by: analyzer.js, index.js

codebase_impact target="src/services/sportsData.js"
# → กระทบ heuristicAnalyzer.js
```

รู้ blast radius ก่อนแก้ → มั่นใจว่าไม่พังที่อื่น

---

## เปรียบเทียบ: SocratiCode vs Manual Exploration

| | Manual (grep/read) | SocratiCode |
|--|-------------------|-------------|
| ค้นหา function | ต้องจำชื่อ | ค้นด้วยภาษาธรรมมา "confidence threshold" |
| ดู dependency | ต้อง grep import ทีละไฟล์ | `graph_query` แสดงทั้งหมด |
| ติดตาม flow | อ่านไล่ทีละไฟล์ | `flow` แสดง call tree |
| Impact analysis | ไม่มี | `impact` แสดง blast radius |
| Onboarding | 30+ นาที | 5 นาที |

---

## ข้อจำกัด

- ต้องมี Docker + Qdrant + Ollama (~500MB RAM)
- ต้อง index ก่อนค้นหา (~1-2 นาที)
- JS symbol detection จำกัด (expression functions อาจ detect ไม่ได้)
- Semantic search ดีแต่ไม่ใช่ magic — query ต้องสื่อสารชัด

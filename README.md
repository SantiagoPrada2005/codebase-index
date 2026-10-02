# 🗺️ Codebase Index (`codebase-index`)

> **Index-First Navigation for AI Agents**: Reduce token consumption by up to **85%**, eliminate hallucinated imports, and enforce zero-drift architectural documentation across your codebase.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![AI Agent Ready](https://img.shields.io/badge/AI%20Agent-Compatible-emerald)](https://github.com/)
[![CI Workflow](https://img.shields.io/badge/CI-Zero--Drift%20Audit-purple)](.github/workflows/index-audit.yml)

---

## ⚡ The Problem: The "Agent Wandering" Tax

When coding agents (Claude Code, Cursor, Antigravity, Windsurf, Codex) work on mid-to-large codebases without structured maps, they suffer from **blind exploration**:

1. **Massive Token Waste**: Grepping and reading 5 to 10 source files just to discover where a function lives burns **5,000–10,000 tokens per search**.
2. **Context Pollution**: Dumping irrelevant source code into the context window degrades reasoning and leads to hallucinated imports.
3. **Architectural Erosion**: Agents place new files in arbitrary locations without understanding layer boundaries.

---

## 🎯 The Solution: Index-First Navigation

`codebase-index` enforces a strict **Index-First Protocol**:

```
[Agent Receives Task]
         │
         ▼
1. Reads nearest INDEX.md (~150 tokens)
   └─ Discovers exact file, architectural role, public exports & couplings
         │
         ▼
2. Opens ONLY the target file (Surgical read)
   └─ Zero wasted tokens, 100% accurate contracts
         │
         ▼
3. Modifies code & updates INDEX.md (Zero-Drift Definition of Done)
```

---

## 📁 Repository Structure

```
codebase-index/
├── SKILL.md                  # Core skill instruction set for AI agents
├── references/
│   └── schema.md             # Canonical markdown schema & field specifications
├── scripts/
│   └── index_manager.py      # Universal CLI for auditing, syncing, and scaffolding
├── .github/
│   └── workflows/
│       └── index-audit.yml   # Ready-to-use GitHub Action for PR CI checks
├── LICENSE                   # MIT License
└── README.md
```

---

## 🚀 Quick Start & Installation

### Option 1: Install as an Agent Skill (Antigravity / Claude Code / Cursor)

Clone this repository directly into your project's `.agents/skills` directory:

```bash
git clone https://github.com/SantiagoPrada2005/codebase-index.git .agents/skills/codebase-index
```

### Option 2: Add CLI Scripts to `package.json`

Add the following scripts to your root `package.json`:

```json
{
  "scripts": {
    "index:audit": "python3 .agents/skills/codebase-index/scripts/index_manager.py audit",
    "index:tree": "python3 .agents/skills/codebase-index/scripts/index_manager.py tree",
    "index:sync": "python3 .agents/skills/codebase-index/scripts/index_manager.py sync-all"
  }
}
```

---

## 🛠️ CLI Tool Reference (`index_manager.py`)

The bundled `index_manager.py` script is **pure Python** (no external dependencies required) and runs on Python 3.8+:

### 1. Audit Project Parity (`audit`)
Verifies that all architectural folders have an `INDEX.md`, checks for unindexed files, and catches deleted/orphan entries:
```bash
python3 scripts/index_manager.py audit
```
*Exits with code `0` on success, or `1` if desynchronization is detected.*

### 2. Scaffold or Update Directory Index (`scaffold`)
Scaffolds a new `INDEX.md` or merges newly created files into an existing index without overwriting existing descriptions:
```bash
python3 scripts/index_manager.py scaffold src/services
```

### 3. Display Architectural Sitemap (`tree`)
Prints a clean architectural map of all indexed modules, their domain layers, and responsibilities:
```bash
python3 scripts/index_manager.py tree
```

### 4. Sync All Indices (`sync-all`)
Re-synchronizes every `INDEX.md` in the project in a single command:
```bash
python3 scripts/index_manager.py sync-all
```

---

## 📋 The `INDEX.md` Schema

Every indexed directory follows a compact, high-density format:

```markdown
# Index: `src/auth`

**Responsibility**: Core authentication, JWT session verification, and OAuth providers.
**Architectural Layer**: Application / Infrastructure

## Subdirectories & Child Modules

| Subdirectory | Responsibility | Index |
| :--- | :--- | :--- |
| [`providers/`](./providers/) | Third-party OAuth providers (Google, GitHub) | [INDEX.md](./providers/INDEX.md) |

## File Manifest

| File | Role / Pattern | Public Exports / API | Key Dependencies |
| :--- | :--- | :--- | :--- |
| [`session.ts`](./session.ts) | Session Manager | `createSession()`, `verifySession()` | `jose`, `cookie` |
| [`guard.ts`](./guard.ts) | Route Guard | `requireAuth()`, `requireRole()` | `astro:middleware` |

## Invariants & Directory Rules

- All session verification must happen in Edge runtime.
- Never expose private keys or secrets to client components.

<!-- Reconciled by codebase-index -->
```

*(See [references/schema.md](references/schema.md) for complete field rules).*

---

## 🤖 Continuous Integration (Zero-Drift CI)

Ensure agents or human developers never forget to update indices. Add the included workflow [`.github/workflows/index-audit.yml`](.github/workflows/index-audit.yml) to your repository:

```yaml
name: Codebase Index Audit

on:
  pull_request:
    branches: [ main, master, develop ]

jobs:
  audit-indexes:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.x'
      - name: Verify Index Parity
        run: python3 scripts/index_manager.py audit
```

If an agent adds, renames, or deletes a file without updating its folder's `INDEX.md`, the PR check immediately fails.

---

## 📄 License

MIT © [Santiago Prada](https://github.com/SantiagoPrada2005)

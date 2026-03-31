# pvnkmnk — AGENTS.md (Vault-as-Runtime Model)

> For any AI agent reading this repository: this document describes the **Unified Vault-as-Runtime Model** —
> the architectural philosophy connecting all pvnkmnk projects into one coherent developer runtime.

---

## What This Repository Is

This is the **global context layer** for all pvnkmnk projects. It contains:

- `GEMINI.md` — primary identity file read by gemini-cli and opencode at session start
- `AGENTS.md` — this file; agent-specific orientation and runtime model
- `docs/` — per-project context notes, the Vault-as-Runtime model, and architecture docs
- `vault-templates/` — Obsidian vault structure templates for the CreativeBrain vault

---

## The Unified Vault-as-Runtime Model

The core insight: a developer's **Obsidian vault is not just personal memory — it is the semantic coordination layer**
across all running systems. When wired correctly, it becomes a living runtime context that any agent can query
and any agent can update.

### The Full Stack

```
LAYER 0 — PHYSICAL INFRASTRUCTURE
  Proxmox HP Laptop (i5-7200U, always-on, 15W)
   └── NullClaw + Navidrome + slskd LXCs       [proxmox-nullclaw-media-server]

LAYER 1 — AGENT RUNTIME
  Homelab-Agents (Docker Agent v3, RTX 3060)
   └── skills/          — auto-generated YAML per managed service
   └── knowledge/       — RAG-indexed docs per service + project context
   └── Docker Model Runner (qwen2.5:7b-instruct, nomic-embed-text)
   └── open-terminal MCP tools                  [Homelab-Agents]

LAYER 2 — PROJECT RUNTIMES
  djinn-netrunner     (Go + SQLite + Fiber)      Music acquisition
  CaterKingOperations (Expo + Supabase)          Business ops
  music-library-manager (Python + Docker)        Library organization
  rentFalcon          (Python + Flask + Selenium) Housing access tool
                                                  [all individual repos]

LAYER 3 — SEMANTIC MEMORY
  Obsidian CreativeBrain Vault
   └── 05 Dev Projects/ — per-project context, session logs, decisions
   └── 01 Music/        — creative work, lyrics, artist profiles
   └── 02 Social Work/  — research, social service notes
   └── obsidian-mcp-server — exposes vault to all agent sessions

LAYER 4 — EXECUTION TOOLS
  opencode            — primary agentic IDE (reads GEMINI.md per project)
  gemini-cli          — primary model CLI (reads GEMINI.md per project)
  Cursor              — secondary IDE with .cursor/ rules per project
```

### The Critical Bridges

**Bridge A — Vault ↔ Homelab-Agents knowledge base:**
Vault project context notes sync into `Homelab-Agents/knowledge/projects/` so the homelab
crew can answer questions about any project's current state.

**Bridge B — Per-project opencode.json MCP configuration:**
The obsidian-mcp-server is added to every project's opencode config so vault context
is available in every coding session.

**Bridge C — Session logs as persistent cross-project memory:**
After every significant session, agents write structured logs to the vault.
Any future agent on any project can retrieve "why was X decided?" by querying vault.

---

## Project Profiles (Agent Orientation)

### rentFalcon
- **Type:** Community tool / social service application
- **Domain:** Housing access, York Region Ontario
- **Stack:** Python 3.13, Flask 3.1, BeautifulSoup, Selenium, APScheduler, SQLAlchemy
- **Origin:** First project that worked. Built in 5 days. Inspired by Inn from the Cold.
- **Key constraint:** Must be usable by non-technical users — ships with .bat launchers and HTML guides
- **Key value:** Free, local, no-accounts, no tracking, open source, Housing First
- **Agent entry:** `README.md` → `APPLICATION_DESCRIPTION.md` → `scrapers/scraper_manager.py`
- **Future:** Docker image (homelab-deployable), Mac/Linux, Discord alerts, map view

### djinn-netrunner
- **Type:** Music acquisition pipeline
- **Stack:** Go, SQLite, Fiber, HTMX, MCP server (20+ tools)
- **Key constraint:** SOCKS5 proxy support required; privacy-first acquisition
- **Agent entry:** `AGENTS.md` → `codemap.md` → `backend/cmd/agent/`

### CaterKingOperations
- **Type:** Live business operations app
- **Stack:** Expo (React Native), Supabase, Drizzle ORM, NativeWind
- **Key constraint:** Real-time KDS (Kitchen Display System); production-grade reliability
- **Agent entry:** `GEMINI.md` (26KB comprehensive context)

### music-library-manager
- **Type:** Library organization service
- **Stack:** Python, Docker, conductor/ service orchestration
- **Key constraint:** Works on FLAC and tagged audio; feeds Navidrome-compatible structure
- **Agent entry:** `GEMINI.md` → `conductor/` → `src/`

### Homelab-Agents
- **Type:** Self-hosted multi-agent AI crew
- **Stack:** Docker Agent v3, LlamaIndex, Ollama, qwen2.5:7b on RTX 3060
- **Key constraint:** Fully offline capable; terminal access via open-terminal MCP
- **Agent entry:** `docker-agent/homelab-team.yaml`

### proxmox-nullclaw-media-server
- **Type:** Infrastructure ops guide + media server
- **Stack:** Proxmox VE, Alpine LXC (NullClaw ~1MB), Navidrome, slskd
- **Key constraint:** Resource-constrained (i5-7200U, 8GB RAM); Discord as off-device UI
- **Agent entry:** `README.md` → `docs/01-proxmox-setup.md`

---

## Developer Fingerprints

Every agent should internalize these:

1. **Docs are first-class** — GEMINI.md, AGENTS.md, codemaps, runbooks are project deliverables
2. **The music pipeline is personal infrastructure** — treat it with the same care as business-critical systems
3. **Social tools are low-tech accessible** — rentFalcon's .bat files and HTML guides are features, not debt
4. **Hardware constraints drive architecture** — if it won't run on an i5-7200U with 8GB RAM, it needs justification
5. **Multi-runtime fluency** — Go, TypeScript, Python, YAML-declarative — no stack tribalism
6. **Privacy is non-negotiable** — no telemetry, no mandatory accounts, no cloud lock-in
7. **Dual power philosophy** — technology built to reduce dependency on landlords, platforms, and intermediaries

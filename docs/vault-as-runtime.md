# The Unified Vault-as-Runtime Model
### pvnkmnk Developer Architecture Document
*Last updated: 2026-03-30*

---

## Overview

This document describes the architectural model connecting all pvnkmnk projects into one coherent
developer runtime — where the Obsidian CreativeBrain vault functions as the **semantic coordination layer**
rather than just a personal note-taking app.

The model emerges from analysis of six active projects:

| Project | Domain |
|---|---|
| rentFalcon | Housing access / social service tool |
| djinn-netrunner | Music acquisition pipeline |
| CaterKingOperations | Catering business operations |
| music-library-manager | Music library organization |
| Homelab-Agents | Self-hosted AI agent crew |
| proxmox-nullclaw-media-server | Always-on media server |

---

## The Developer Profile

**jd gramsci** is a Sociologist, Social Service Worker, Independent Musician, and Developer in Newmarket, ON.
He builds tools that serve real needs: a housing aggregator for clients navigating York Region's rental market,
a catering operations system for a real business, a personal music pipeline spanning acquisition to playback,
and a self-hosted homelab that keeps everything off the cloud.

**Key developer signals:**
- Privacy-first: no project introduces cloud-mandatory dependencies
- Operator-class documentation: every repo has structured docs for human and AI readers
- Multi-runtime fluency: Go, TypeScript, Python, YAML — no stack tribalism
- Hardware-constrained optimization: RTX 3060 (8GB VRAM) for AI; i5-7200U (15W) for always-on ops
- Social purpose: "dual power where ever I can" (rentFalcon README acknowledgments)
- First project that worked (rentFalcon) built in 5 days for community need, not portfolio

---

## The Full Stack Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│ LAYER 0 — PHYSICAL INFRASTRUCTURE                                   │
│                                                                     │
│  Proxmox HP Laptop (i5-7200U, 8GB RAM, 15W, always-on)            │
│   ├── Alpine LXC: NullClaw (~1MB RAM, Zig binary)                  │
│   ├── Debian LXC: Navidrome (~150MB RAM)                           │
│   ├── Debian LXC: slskd (~100MB RAM)                               │
│   └── WD 1TB External SSD (USB 3.1) — all media/data              │
│   → Discord Command Center (off-device UI, zero local RAM)         │
│                                           [proxmox-nullclaw]        │
├─────────────────────────────────────────────────────────────────────┤
│ LAYER 1 — AGENT RUNTIME                                             │
│                                                                     │
│  Homelab-Agents (main workstation, RTX 3060 8GB)                   │
│   ├── Docker Agent v3 runtime                                       │
│   ├── skills/          YAML skill files per managed service         │
│   ├── knowledge/       RAG-indexed docs (LlamaIndex + nomic-embed) │
│   ├── qwen2.5:7b-instruct via Docker Model Runner                  │
│   └── open-terminal + terminals (MCP tools for shell access)       │
│                                           [Homelab-Agents]          │
├─────────────────────────────────────────────────────────────────────┤
│ LAYER 2 — PROJECT RUNTIMES                                          │
│                                                                     │
│  djinn-netrunner  Go + SQLite + Fiber + HTMX + MCP server         │
│  CaterKingOps     Expo + Supabase + Drizzle + NativeWind           │
│  music-lib-mgr    Python + Docker + conductor/                      │
│  rentFalcon       Python + Flask + Selenium + BeautifulSoup        │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│ LAYER 3 — SEMANTIC MEMORY (THE VAULT)                               │
│                                                                     │
│  Obsidian CreativeBrain Vault                                       │
│   ├── 01 Music/        Creative work, lyrics, artist profiles      │
│   ├── 02 Social Work/  Research, case frameworks, org notes        │
│   ├── 05 Dev Projects/ Context notes + session logs per project    │
│   └── obsidian-mcp-server → exposes vault to all agent sessions    │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│ LAYER 4 — EXECUTION TOOLS                                           │
│                                                                     │
│  opencode   (reads GEMINI.md per project)                          │
│  gemini-cli (reads GEMINI.md per project)                          │
│  Cursor     (reads .cursor/ rules per project)                     │
│                                                                     │
│  All three tools see: project GEMINI.md + global GEMINI.md        │
│  Bridge needed: connect Obsidian vault to all agent sessions       │
└─────────────────────────────────────────────────────────────────────┘
```

---

## The Three Critical Bridges

### Bridge A — Vault ↔ Homelab-Agents Knowledge Base

The Homelab-Agents repo has a `knowledge/services/` directory already indexed by RAG.
Extending it to include project context notes creates a crew that can answer
"what is the current status of djinn?" using vault memory as its source.

```bash
# Sync vault project context into Homelab-Agents knowledge base
rsync ~/Obsidian/CreativeBrain/"05 Dev Projects"/ \
      ~/Homelab-Agents/knowledge/projects/ \
      --include="*-context.md" --include="*-service-map.md" \
      --exclude="Session Logs/" -r --delete
```

```yaml
# skills/vault-sync.yaml — Homelab crew writes back to vault
name: vault-sync
description: Update Obsidian vault service-map notes with current homelab state
triggers: [scheduled, on_health_check]
steps:
  - check all container health via Docker API
  - format markdown summary with status, uptime, last error
  - write to ~/Obsidian/.../proxmox-nullclaw/nullclaw — service map.md
  - write to ~/Obsidian/.../Homelab-Agents/homelab — service map.md
```

### Bridge B — Per-Project opencode.json MCP Configuration

Add obsidian-mcp-server to every project's opencode config:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "obsidian-vault": {
      "type": "local",
      "command": ["obsidian-mcp-server", "--vault", "~/Obsidian/CreativeBrain"],
      "enabled": true
    }
  }
}
```

For djinn-netrunner, also expose its built-in MCP server:

```json
{
  "mcp": {
    "obsidian-vault": { "...": "as above" },
    "djinn-agent": {
      "type": "local",
      "command": ["./netrunner-agent"],
      "enabled": true
    }
  }
}
```

### Bridge C — Session Logs as Persistent Cross-Project Memory

**Log location:** `~/Obsidian/CreativeBrain/05 Dev Projects/<project>/Session Logs/YYYY-MM-DD-<task>.md`

**Log format:**
```markdown
## Session: YYYY-MM-DD — <task summary>
**Project:** <project name>
**Duration:** ~Xh
**What was done:** <bullet list>
**Decisions made + rationale:** <bullet list>
**Blockers / open questions:** <bullet list>
**Next steps:** <bullet list>
**Cross-project impacts:** <any effects on other projects>
```

---

## The Music Pipeline as Unified Semantic Thread

```
djinn-netrunner
  ↓ acquires audio files (Go, SOCKS5-proxied, SQLite queue)
music-library-manager
  ↓ organizes + tags (Python, conductor/ service, FLAC-aware)
proxmox-nullclaw (Navidrome)
  ↓ serves library (always-on, Newmarket LAN + Tailscale)
Obsidian CreativeBrain (01 Music/)
  ↔ holds creative intent, lyrics, artist research, session memory
```

A quality preference in vault ("prioritize FLAC over speed") should inform djinn's acquisition logic.
A new artist discovered via slskd should propagate to vault artist notes.
The pipeline is personal infrastructure — treat it accordingly.

---

## The Social Service Thread

rentFalcon establishes a pattern for all future social service tooling:

| Principle | rentFalcon Implementation |
|---|---|
| Local-first | Flask app runs on localhost, no cloud dependency |
| Non-technical accessible | EASY_SETUP.bat, HOW_TO_START.html, USER_GUIDE_SIMPLE.md |
| Free, always | MIT license, no ads, no subscriptions, no accounts |
| Privacy-preserving | No data collection, searches stay local |
| Community-rooted | Newmarket + York Region specifically, Housing First acknowledgment |
| Built fast for real need | 5 days development, production-ready |

Future tools in this thread (shelter waitlist trackers, food bank finders, benefits navigators)
should inherit all of these constraints.

---

## The Global GEMINI.md Hierarchy

```
Global GEMINI.md          → who am I, what are my constraints, what is my full project map
  └── Project GEMINI.md   → what is this specific project, what is the current sprint
        └── Session logs  → what happened last time, what decisions were made
```

---

## Vault Folder Structure

```
~/Obsidian/CreativeBrain/
├── 00 Inbox/                    ← capture anything here first
├── 01 Music/
│   ├── Songs/                   ← one note per song (lyrics, status, links)
│   ├── Artists/                 ← artist profiles + research
│   ├── Releases/                ← album/EP planning
│   └── Sessions/                ← recording/production session logs
├── 02 Social Work/
│   ├── Research/                ← sociology notes, frameworks, papers
│   ├── Organizations/           ← orgs, contacts, resources
│   └── Case Frameworks/         ← de-identified practice frameworks
├── 03 Catering/                 ← CaterKing business notes
├── 04 Personal/
├── 05 Dev Projects/
│   ├── rentFalcon/
│   │   ├── rentfalcon — context.md
│   │   ├── rentfalcon — roadmap.md
│   │   └── Session Logs/
│   ├── djinn-netrunner/
│   │   ├── djinn — context.md
│   │   ├── djinn — architecture.md
│   │   └── Session Logs/
│   ├── CaterKingOperations/
│   │   ├── caterking — context.md
│   │   └── Session Logs/
│   ├── music-library-manager/
│   │   ├── mlm — context.md
│   │   └── Session Logs/
│   ├── Homelab-Agents/
│   │   ├── homelab — context.md
│   │   ├── homelab — skills registry.md
│   │   ├── homelab — service map.md      ← auto-updated by agents
│   │   └── Session Logs/
│   └── proxmox-nullclaw/
│       ├── nullclaw — context.md
│       ├── nullclaw — service map.md     ← auto-updated by agents
│       └── Session Logs/
├── 06 Sociology/
├── 07 Meta/
│   ├── Templates/
│   └── Vault Setup Guide.md
└── 08 Archive/
```

---

## What Changes When Fully Wired

| Before | After |
|---|---|
| Each project agent knows only its own GEMINI.md | Every agent knows full developer identity, constraints, cross-project map |
| Session decisions evaporate with context window | Session logs accumulate; agents retrieve "why was X decided?" from vault |
| Homelab crew manages services but not projects | Homelab crew can answer questions about any project's current state |
| Obsidian vault is personal notes only | Vault is the semantic coordination plane for the entire stack |
| Music pipeline tools work in isolation | Pipeline tools share intent (quality prefs, discovery) via vault bridge |
| rentFalcon is isolated community tool | rentFalcon establishes the pattern for all future social service tools |
| 6 isolated agent contexts | 1 unified developer runtime with persistent, compounding cross-project memory |

# pvnkmnk — Developer Runtime Context (GEMINI.md)

> This file is the **global identity context** for all AI agents working in pvnkmnk's projects.
> Read this first. Every tool, every session, every project.

---

## Identity

- **Name:** jd gramsci (pvnkmnk)
- **Location:** Newmarket, Ontario, Canada
- **Roles:** Sociologist · Social Service Worker · Independent Musician/Artist · Developer
- **Philosophy:** Privacy-first. Locally-owned. Operator-class documentation. Dual power where possible.
- **Inspiration:** Inn from the Cold (housing justice) · Working-class mutual aid · Self-hosted sovereignty

---

## Active Project Ecosystem

| Project | Runtime | Domain | Agent Entry Point |
|---|---|---|---|
| djinn-netrunner | Go + SQLite + Fiber + HTMX | Music acquisition pipeline | `AGENTS.md` + `codemap.md` |
| CaterKingOperations | Expo + Supabase + Drizzle + NativeWind | Catering business ops | `GEMINI.md` (26KB) |
| music-library-manager | Python + Docker + conductor/ | Music library organization | `GEMINI.md` + `README.md` |
| Homelab-Agents | Docker Agent v3 + LlamaIndex + Ollama | Self-hosted homelab AI crew | `docker-agent/homelab-team.yaml` |
| proxmox-nullclaw-media-server | Proxmox + Alpine LXC + Zig (NullClaw) | Personal media server | `README.md` |
| rentFalcon | Python + Flask + Selenium + BeautifulSoup | Housing access tool (social service) | `README.md` + `APPLICATION_DESCRIPTION.md` |

---

## Hardware Context

| Machine | Role | Specs |
|---|---|---|
| Main workstation | Dev + Homelab-Agents runtime | Windows 11 + WSL2, NVIDIA RTX 3060 8GB VRAM |
| HP 15-bs0xx (Proxmox) | Always-on media/ops server | i5-7200U, 8GB RAM, 1TB HDD + 1TB WD SSD (USB 3.1), 15W TDP |

All inference runs locally on RTX 3060 — no cloud APIs required (cloud fallback optional).

---

## Universal Constraints

These apply to **every project**, no exceptions:

1. **No cloud-mandatory dependencies** — if it requires a paid cloud service to function, reject it
2. **Minimal footprint** — prefer single binaries, Alpine containers, lean runtimes; optimize for constrained hardware
3. **SOCKS5/proxy support** — where network traffic is involved, proxy support is expected
4. **Logs are the UI** — meaningful, structured output over progress spinners and silent failures
5. **Privacy by default** — no telemetry, no tracking, no accounts where not strictly necessary
6. **Document for agents** — every significant project has AGENTS.md / GEMINI.md / codemap.md for AI-first onboarding
7. **Deployable by non-technical users** — tools with community impact (rentFalcon, future social service tools) must be accessible

---

## The Music Pipeline (Cross-Project Thread)

Music flows through the entire stack as a unified pipeline. Any agent touching music tooling must understand the full chain:

```
djinn-netrunner      → acquires music (Go, SOCKS5-proxied, SQLite queue)
        ↓
music-library-manager → organizes + tags library (Python, Docker, conductor/ service)
        ↓
proxmox-nullclaw      → serves music (Navidrome + slskd on Alpine LXC, always-on)
        ↓
Obsidian CreativeBrain → holds creative context, lyrics, artist notes, session memory
```

A preference expressed in the vault (e.g. "prioritize FLAC quality") should inform djinn's acquisition settings. A new artist discovered via djinn should propagate to vault artist notes.

---

## The Social Service Thread (rentFalcon + Future Tools)

rentFalcon is the first project that *worked* and is rooted in direct community need — housing access in York Region, Ontario. It was built in 5 days. Key signals for any agent:

- Users are often **non-technical** — rentFalcon ships with `EASY_SETUP.bat`, a desktop shortcut creator, and an HTML quick-start guide
- The tool is explicitly **free, ad-free, no-accounts** — monetization is a non-goal
- Documentation exists at **multiple literacy levels** (developer README, `USER_GUIDE_SIMPLE.md`, `HOW_TO_START.html`)
- Future social service tools should follow the same pattern: local, free, accessible, documented for real humans

---

## Post-Session Protocol

After any significant development session, write a log:

```
~/Obsidian/CreativeBrain/05 Dev Projects/<project>/Session Logs/YYYY-MM-DD-<task>.md
```

Log format:
```markdown
## Session: YYYY-MM-DD — <task>
**Duration:** ~Xh
**What was done:**
**Decisions made + rationale:**
**Blockers / open questions:**
**Next steps:**
```

---

## MCP Tools Available (Global)

- `obsidian-mcp-server` — vault at `~/Obsidian/CreativeBrain`
- `djinn-agent` — when inside djinn-netrunner project (`backend/cmd/agent/`)
- `homelab Docker Agent` — when managing infrastructure (`docker-agent/homelab-team.yaml`)
- `open-terminal` / `terminals` — registered via `scripts/register_terminal.sh` in Homelab-Agents

---

## Agent Collaboration Notes

- This developer writes **AGENTS.md and GEMINI.md as first-class project artifacts** — treat them as ground truth for project context
- Cursor, opencode, and gemini-cli are the primary coding interfaces
- Prefer **reasoning out loud in session logs** over silent decisions
- When uncertain about scope, err toward doing less and asking — this developer has strong opinions about project direction

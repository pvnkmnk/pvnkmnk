# Cross-Project Relationship Map

*How all pvnkmnk projects connect to each other.*

---

## Data Flows

```
                        ┌──────────────────┐
                        │  djinn-netrunner │
                        │  (music acq.)    │
                        └────────┬─────────┘
                                 │ acquired audio files
                                 ▼
                    ┌────────────────────────┐
                    │  music-library-manager │
                    │  (organizes + tags)    │
                    └────────────┬───────────┘
                                 │ Navidrome-compatible library
                                 ▼
              ┌──────────────────────────────────────┐
              │  proxmox-nullclaw (Navidrome + slskd) │
              │  (serves music, always-on)            │
              └──────────────────────────────────────┘
                                 │
                        ┌────────▼─────────┐
                        │  Obsidian Vault  │
                        │  (creative layer)│
                        └──────────────────┘

  ┌─────────────────┐         ┌──────────────────────┐
  │  rentFalcon     │         │  CaterKingOperations  │
  │  (housing tool) │         │  (catering ops)       │
  └────────┬────────┘         └──────────┬────────────┘
           │                              │
           │     both feed               │
           └──────────┬───────────────────┘
                      │ session logs + context
                      ▼
           ┌──────────────────────┐
           │  Obsidian Vault      │
           │  05 Dev Projects/    │
           └──────────┬───────────┘
                      │ knowledge/projects/ sync
                      ▼
           ┌──────────────────────┐
           │  Homelab-Agents      │
           │  (knows all projects)│
           └──────────────────────┘
```

---

## Dependency Matrix

| Project | Depends On | Feeds Into |
|---|---|---|
| djinn-netrunner | SOCKS5 proxy, SQLite | music-library-manager |
| music-library-manager | djinn output | proxmox-nullclaw (Navidrome) |
| proxmox-nullclaw | music-library-manager output | Discord Command Center |
| Homelab-Agents | RTX 3060, Docker, Ollama | manages all homelab services |
| rentFalcon | Chrome, internet | future: proxmox-nullclaw (deployment) |
| CaterKingOperations | Supabase (cloud), Expo | — |

---

## Shared Patterns Across Projects

| Pattern | Projects |
|---|---|
| GEMINI.md / AGENTS.md context files | djinn, CaterKing, music-lib-mgr, Homelab-Agents, this repo |
| Docker-based deployment | music-lib-mgr, Homelab-Agents, rentFalcon (planned v3.0) |
| Local-first, no mandatory cloud | djinn, rentFalcon, Homelab-Agents, proxmox-nullclaw |
| Non-technical user accessibility | rentFalcon (primary), future social tools |
| Discord as off-device UI | proxmox-nullclaw, djinn (planned), rentFalcon (planned v2.2) |
| Parallel/async processing | rentFalcon (scrapers), music-lib-mgr (conductor), djinn (queue) |

---

## The Social Service Thread

rentFalcon is the seed of a broader toolset for community support work:

```
rentFalcon (v2.1, active)
  ↓ establishes pattern: local, free, accessible, documented
future: shelter-finder
future: benefits-navigator
future: food-bank-locator
future: [whatever York Region needs next]
```

All future tools in this thread inherit rentFalcon's constraints:
- Run locally (no SaaS)
- Free and MIT licensed
- Accessible to non-technical users
- Documented at multiple literacy levels
- No accounts, no tracking

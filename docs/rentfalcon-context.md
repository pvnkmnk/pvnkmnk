# rentFalcon — Project Context Note

> Vault context note for the Obsidian CreativeBrain vault.
> Read by Homelab-Agents crew via knowledge/projects/ sync.

---

## What It Is

rentFalcon is a rental listing aggregator for York Region, Ontario. It searches Kijiji and Rentals.ca
simultaneously and presents deduplicated results in a clean local web interface.

**Tagline:** *get the swoop on landlords*

It was the **first project that fully worked**. Built in 5 days. Inspired by Inn from the Cold
and the principle of building dual power — reducing tenant dependency on landlord-controlled platforms.

## Stack

- Python 3.13, Flask 3.1
- BeautifulSoup (Kijiji scraping — HTML/JSON-LD)
- Selenium + ChromeDriver (Rentals.ca — JS-rendered)
- APScheduler, Flask-SQLAlchemy
- Vanilla HTML/CSS/JS frontend

## Key Design Decisions

- **No accounts, no cloud, no tracking** — runs entirely on localhost, MIT licensed
- **Non-technical user accessibility** — EASY_SETUP.bat, CREATE_DESKTOP_SHORTCUT.bat,
  HOW_TO_START.html, USER_GUIDE_SIMPLE.md — multiple literacy levels documented
- **York Region specific** — Newmarket as center, 8 cities within 25km radius
- **Parallel scraping** — all sources run simultaneously, ~10-15s for 30-50 results
- **85% similarity deduplication** — removes cross-source duplicates automatically

## Active Scrapers

| Source | Method | Speed | Status |
|---|---|---|---|
| Kijiji | HTML/JSON-LD parsing | 3-5s | ✅ Active |
| Rentals.ca | Selenium automation | 10-15s | ✅ Active |
| Realtor.ca | API blocked | N/A | ⚠️ Disabled |

## Roadmap

**v2.2:** Facebook Marketplace scraper · Saved searches · Discord/Telegram/email alerts · Mac/Linux support

**v3.0:** Docker image (priority — enables homelab deployment) · Map view · Expand beyond York Region · Mobile app

## Connection to Other Projects

- **Homelab-Agents:** v3.0 Docker image should be deployable via homelab crew's ServiceOps
- **proxmox-nullclaw:** Could run as an LXC on the always-on Proxmox machine (very low resource footprint)
- **Social Work context:** Future tools (shelter finders, benefit navigators) inherit rentFalcon's
  accessibility and privacy patterns

## Status

Production-ready at v2.1. Primary gap is Docker packaging for homelab deployment.

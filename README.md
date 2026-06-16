# CFMS — Cloudflare Management System

**CFMS** is a self-hosted browser dashboard for **Cloudflare tunnels** and **zone operations**. One FastAPI service and one web UI centralize hostname routing, tunnel operations, DNS, cache controls, security checks, and WordPress hardening workflows.

Private source: [logicencoder/cfms](https://github.com/logicencoder/cfms). This overview describes what the UI can do — credentials and zone IDs stay in your local `.env`.

## Tech stack

| Layer | Technologies |
|-------|--------------|
| Backend | FastAPI + uvicorn |
| Cloudflare API | httpx + JSON APIs + GraphQL analytics |
| Config layer | YAML parsing for `cloudflared` tunnel config |
| Frontend | Single-page HTML dashboard |
| Live updates | WebSocket push from backend |

## Tunnel operations in one panel

CFMS shows live tunnel endpoint health, connections, and request counters, then lets operators edit ingress entries directly from the dashboard. You can add/remove hostnames, edit raw `config.yml`, create backups before writes, and reload or restart the tunnel service without switching tools.

This removes a fragile workflow where tunnel changes were previously split across shell sessions, manual file edits, and separate Cloudflare pages.

## Security and WordPress hardening

CFMS groups Cloudflare security controls into operator workflows: rulesets, firewall/access rules, rate limits, page rules, bot settings, and recent security events. It also includes WordPress-focused presets for common attack surfaces like login and admin routes.

A built-in multi-user-agent access test checks real behavior after rule changes, so operators can verify that intended traffic is allowed while challenge/block rules still trigger for risky paths.

## DNS, cache, analytics, and SSL controls

CFMS includes full DNS record management (create/read/update/delete), cache purge tools (global or URL list), seven-day analytics summaries, and TLS controls in the same interface.

This helps during incidents where routing, cache state, and SSL posture must be adjusted quickly without context switching.

## Operator safety features

Config backups are part of the write flow, and a global "Hide Sensitive Data" mode masks secret values on screen. This is useful for screen-sharing support sessions where operators need to demonstrate setup without exposing tokens.

## Quick start

FastAPI backend with **httpx**, **orjson**, and WebSocket push; frontend is one **`cfms.html`** file — no separate Node build step.

```bash
pip install fastapi uvicorn httpx orjson psutil pyyaml
cp .env.example .env   # Cloudflare tokens, zone ID, tunnel paths
python3 cfms.py        # default http://localhost:5055
```

See the private repo README for environment variables and [REPOS.md](REPOS.md) for repository links.

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)

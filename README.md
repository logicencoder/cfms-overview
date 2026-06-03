# CFMS — Cloudflare Management System (overview)

Operator dashboard for **Cloudflare tunnels** and **zone controls** on the LogicEncoder SOL server. One browser UI replaces ad-hoc `config.yml` edits, scattered `curl` tests, and the Cloudflare dashboard tabs you need during incident response.

**Code (private):** [logicencoder/cfms](https://github.com/logicencoder/cfms)  
**Delivery:** SOL only — `http://<sol>:5055` via `cfms.service` (not a public logicencoder.com app)

Screenshots: *to be added.*

---

## What

| Area | Capability |
|------|------------|
| **Tunnel** | List ingress endpoints, live health, add/remove hostnames in `config.yml`, backup/restore, reload/restart/stop/start `cloudflared` |
| **Security** | Zone settings, WAF custom rules, legacy firewall, rate limits, IP rules, page rules, bot management, security events (24h) |
| **WordPress ops** | WP hardening presets (`/wp-login.php`, `/wp-admin`, optional `xmlrpc`), bypass-rule patch, multi–User-Agent live tests |
| **DNS & cache** | Full DNS CRUD, purge all or by URL |
| **Insight** | 7-day traffic analytics, SSL/TLS toggles, CF trace per UA |
| **Operator safety** | Config backups before every write; global “hide sensitive data” for screen sharing |

**Stack:** FastAPI + orjson + httpx + WebSockets; UI in `cfms.html` (no separate frontend build).

---

## Why

LogicEncoder runs many services through **Cloudflare tunnels** and a single zone. Changing ingress or testing bot challenges used to mean SSH, manual YAML, and API calls. CFMS centralises that with realtime status and logs so tunnel mistakes are visible before they take production down.

---

## Who

- **Primary user:** operator maintaining SOL + Hostinger + tunnel routes  
- **Not for:** public site visitors — internal infrastructure tool  
- **Integrates with:** `universal-service-manager`, gas/MEXC backends exposed via tunnel hostnames documented in each app’s ARCHITECTURE

---

## Run (summary)

Copy `.env.example` → `.env` in the private repo, install deps from [cfms README](https://github.com/logicencoder/cfms/blob/main/README.md), `python3 cfms.py`. Production uses systemd on SOL — see private `ARCHITECTURE.md`.

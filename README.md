# CFMS — Cloudflare Management System

**CFMS** is a self-hosted browser dashboard for **Cloudflare tunnels** and **zone operations**. One FastAPI app and a single HTML page replace juggling the Cloudflare dashboard, hand-edited `config.yml` files, and one-off API curls when you add a hostname, test a bot challenge, or purge cache during an incident.

Private source: [logicencoder/cfms](https://github.com/logicencoder/cfms). This overview describes what the UI can do — credentials and zone IDs stay in your local `.env`.

## The problem it solves

Teams that expose homelab or production services through **cloudflared** often touch the same tasks repeatedly: add an ingress hostname, reload the tunnel, confirm DNS, tighten WordPress login paths, or check whether a WAF rule still fires. CFMS puts tunnel health, config edits, backups, and Cloudflare API controls in one place with live status over WebSockets.

## Tunnel management

See every tunnel endpoint with live health, connection counts, and request stats. Add or remove hostnames in `config.yml`, edit the raw file when you need precision, create backups before writes, and reload, restart, stop, or start the tunnel service from the UI.

## Cloudflare security and WordPress hardening

Browse zone settings, custom WAF rulesets, legacy firewall rules, rate limits, IP access rules, page rules, and bot management toggles. Review security events from the last 24 hours.

WordPress-focused presets protect `/wp-login.php` and `/wp-admin/*`, optionally block `/xmlrpc.php`, patch the “bypass challenge for all bots” expression, and run **live access tests** against multiple User-Agents in parallel so you can see what visitors and crawlers actually hit.

## DNS, cache, analytics, and SSL

Full DNS record CRUD (A, AAAA, CNAME, MX, TXT, NS, SRV, CAA). Purge the entire cache or specific URLs. View seven-day traffic analytics — requests, pageviews, bandwidth, cache hit rate, HTTPS share, threats, and unique visitors — plus per-day breakdowns.

Adjust SSL/TLS mode, minimum TLS version, Always Use HTTPS, HTTP/3, TLS 1.3, and automatic HTTPS rewrites from the same panel. Run **CF Trace** (`/cdn-cgi/trace`) per User-Agent when debugging routing.

## Operator safety

Every config save can go through automatic backups. A global **Hide Sensitive Data** toggle masks API tokens and keys on screen — useful when sharing your desktop during support or streams.

## Stack and quick start

FastAPI backend with **httpx**, **orjson**, and WebSocket push; frontend is one **`cfms.html`** file — no separate Node build step.

```bash
pip install fastapi uvicorn httpx orjson psutil pyyaml
cp .env.example .env   # Cloudflare tokens, zone ID, tunnel paths
python3 cfms.py        # default http://localhost:5055
```

See the private repo README for environment variables and [REPOS.md](REPOS.md) for repository links.

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)

# CFMS — Cloudflare Management System

**One browser console for Cloudflare Tunnels and zone control — live health, ingress editing, DNS, cache, security rules, WordPress hardening, and multi-user-agent verification without leaving the dashboard.**

**CFMS** (Cloudflare Management System) is a self-hosted operator dashboard that unifies **`cloudflared` tunnel routing** and **Cloudflare zone operations** in a single FastAPI app. Instead of juggling shell edits, the Cloudflare web UI, and separate health checks, you monitor tunnel health, change ingress hostnames, manage DNS and cache, tune security rules, harden WordPress paths, and verify access — all with live WebSocket updates.

Built for **operators who run private apps behind Cloudflare Tunnel** and need fast, repeatable changes during deploys and incidents. Credentials stay in your local `.env`; this overview describes product behaviour only.

**Made by [Logic Encoder](https://logicencoder.com)**

Private source: [logicencoder/cfms](https://github.com/logicencoder/cfms)

---

## What you can do

| Area | In plain language |
|------|-------------------|
| **Tunnel overview** | See PID, uptime, memory, CPU, active streams, and per-hostname UP/DOWN with HTTP codes and request counters |
| **Ingress CRUD** | Add or remove hostnames, edit raw `config.yml`, preview YAML, auto-backup every write |
| **Service control** | Reload (`SIGHUP`), start, stop, or restart the tunnel service from the header |
| **Live logs** | Tail `cloudflared` service logs in the browser with colourised levels |
| **Zone security** | View firewall rules, WAF packages, rate limits, IP rules, page rules, and recent security events |
| **WordPress hardening** | Apply managed-challenge presets on login/admin, optional xmlrpc block, patch bot-bypass paths |
| **Access testing** | Parallel URL × User-Agent matrix — see OK vs BLOCKED/challenge after rule changes |
| **DNS & cache** | Full DNS CRUD, proxied toggle, zone-wide or URL-list cache purge |
| **Analytics & SSL** | Seven-day traffic summary, TLS mode, min TLS, Always HTTPS, HTTP/3 |
| **Operator safety** | Hide sensitive mode masks tokens, zone IDs, and IPs for screen sharing |

Everything critical refreshes over **WebSocket** — tunnel status every 15 seconds and a continuous log stream — with REST fallback on manual refresh.

---

## Feature examples (two per capability)

#### Live tunnel overview
1. One hostname returns HTTP 502 while others are UP — you immediately know which public URL is broken without opening Prometheus.
2. Active streams spike after a deploy; you correlate with per-host request counters from the same table.

#### Endpoint inventory
1. You audit all tunnel hostnames, backend ports, and `noTLSVerify` flags before a security review.
2. You remove a decommissioned subdomain from ingress; config is backed up and the tunnel reloads.

#### Add endpoint workflow
1. A new internal app on `localhost:8100` needs public hostname `api.example.com` — you fill the form, preview YAML, and save.
2. WordPress behind the tunnel needs `httpHostHeader` set to the public domain — you add it in the optional field.

#### Raw config editing
1. You paste a multi-rule ingress block from docs into Edit Config and save after YAML validation passes.
2. A typo is caught before write; the error message appears and the previous config stays intact.

#### Config backups
1. Before a risky change, you click **Backup Now** and see a new timestamped file in the list.
2. Every add/remove/save automatically creates a backup — you can roll back manually from the filesystem if needed.

#### Tunnel reload vs restart
1. You edit ingress and hit **Reload** (SIGHUP) for a fast config pick-up without dropping all connections.
2. `cloudflared` is wedged; you use **Restart** from the header after fixing config.

#### Live log tail
1. A hostname shows DOWN; you switch to Live Logs and spot TLS handshake errors in real time.
2. You share screen during incident response with auto-scroll on, watching new connection attempts as they happen.

#### Zone status at a glance
1. You confirm Bot Fight Mode is OFF and security level is not "under attack" before enabling stricter rules.
2. After enabling Bot Management JS elsewhere, you refresh Zone Status to verify `enable_js` flipped ON.

#### Firewall rules visibility
1. You review custom ruleset expressions to find which rule challenges `/wp-admin`.
2. You compare legacy firewall rules vs new ruleset rules during a Cloudflare migration.

#### Security event triage
1. Spike in blocks — you sort Security Events by path and see brute-force attempts on `/wp-login.php`.
2. You identify a false positive: a legitimate crawler blocked with rule ID visible for Cloudflare dashboard follow-up.

#### Live access test (multi-UA)
1. After WP hardening, you run default URLs and confirm Googlebot gets OK on `/blog/` but Python Requests gets BLOCKED on `/`.
2. You test a comma-separated staging URL list and see which User-Agents receive challenge pages (403 + challenge HTML).

#### Bot Management JS toggle
1. Search crawlers fail after JS challenge rollout — you disable Bot JS from CF Controls and re-run Live Test.
2. You re-enable Bot JS before re-opening a public API to reduce automated abuse.

#### WordPress hardening preset
1. You apply hardening with your office IP on the allowlist; `/wp-login.php` challenges everyone else.
2. You enable "block xmlrpc.php" to stop XML-RPC brute-force without touching WordPress plugins.

#### Bot bypass rule maintenance
1. A new `/applications/` section gets challenged for bots — you click **Patch Bypass Rule** to refresh canonical path expression.
2. After Cloudflare UI edit drifted the bypass expression, you restore the known-good skip rule from CFMS.

#### DNS record management
1. You add a CNAME for `staging.example.com` pointing at the tunnel hostname with proxied ON.
2. You edit an A record TTL and toggle from proxied to DNS-only for a mail-related subdomain.

#### Cache purge
1. After a major WordPress theme deploy, you purge listed URLs for CSS/JS assets only.
2. Cache is serving a stale error page — you confirm with modal and purge entire zone cache.

#### Traffic analytics
1. You check seven-day cache hit % dropped after an API-heavy release.
2. Threat count spikes on one day — you cross-check with Security Events from the same period.

#### SSL / TLS controls
1. You set min TLS to 1.2 and enable Always HTTPS from the SSL tab after an audit finding.
2. You toggle HTTP/3 on to test QUIC without leaving the CFMS UI.

#### CF Trace diagnostics
1. You run CF Trace and see ClaudeBot gets a different bot score than Normal Browser on the same domain.
2. Edge IP and Ray ID from trace help correlate with a specific blocked request in Security Events.

#### WAF, rate limit, IP, and page rules
1. You view which rate limit triggers on the login path; confirm an IP allowlist entry exists for your monitoring server.
2. You scan page rules for a stale "cache everything" pattern before enabling a new API route.

#### Settings and operator safety
1. You update `CLOUDFLARE_API_TOKEN_EDIT` in Settings without restarting — credentials reload in-process.
2. You enable **Hide sensitive** before a walkthrough; zone ID and admin IPs show as masked placeholders.

#### Header quick actions
1. One-click **Stop** during a maintenance window; **Start** when origin is ready again.
2. **Refresh** pulls a one-shot status snapshot when WebSocket is temporarily disconnected.

---

## What it does not do

- **Not** a multi-tenant SaaS — one operator deployment with your tokens and zone
- **Not** a replacement for Cloudflare's full dashboard on every edge case — focused on tunnel + common zone ops
- **Not** built-in app authentication — intended behind network access control on a trusted network
- **Not** automatic DNS for new tunnels — you still create records when routing requires it

Tokens, zone IDs, and tunnel paths stay in your local `.env` — not published in this overview repo.

---

## Tech stack

| Layer | Technologies |
|-------|----------------|
| Runtime | Python 3 |
| API server | FastAPI + uvicorn (default port 5055) |
| JSON | orjson (responses + WebSocket messages) |
| Cloudflare | httpx async — REST v4 + GraphQL analytics/events |
| Tunnel config | PyYAML read/write of `cloudflared` ingress YAML |
| Host metrics | psutil (process + TCP connection counts) |
| Health checks | async curl per hostname; DNS resolve with in-memory cache |
| Tunnel metrics | scrape `cloudflared` Prometheus endpoint |
| Service control | systemctl + journalctl + SIGHUP |
| Frontend | Single `cfms.html` — vanilla JS/CSS, no Node build |
| Real-time | WebSocket status broadcast + log stream |

---

## Quick start

```bash
pip install fastapi uvicorn httpx orjson psutil pyyaml
cp .env.example .env   # Cloudflare tokens, zone ID, tunnel paths
python3 cfms.py        # default http://localhost:5055
```

See the private repo README for environment variables and [REPOS.md](REPOS.md) for repository links.

---

## Related repositories

| Repository | Role |
|------------|------|
| [cfms](https://github.com/logicencoder/cfms) | Private application code |
| [cfms-overview](https://github.com/logicencoder/cfms-overview) | This product overview |

See [REPOS.md](REPOS.md).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)

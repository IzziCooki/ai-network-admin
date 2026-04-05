---
name: reverse-proxy-advisor
description: Guides configuration of reverse proxies (Nginx, Caddy, Traefik) and Cloudflare Tunnels for exposing self-hosted services securely.
argument-hint: [proxy-details]
---

# Reverse Proxy Advisor

Use this skill when the user needs to expose a self-hosted service externally, configure a reverse proxy, set up Cloudflare Tunnels, or troubleshoot proxy/tunnel connectivity issues.

## Workflow

1. Identify the goal: expose a service externally, add TLS, consolidate services behind one domain, or troubleshoot an existing proxy/tunnel.
2. Confirm environment: what proxy/tunnel software is in use (or preferred), upstream service and port, domain name, DNS provider.
3. Confirm network path: is the server behind NAT/CGNAT? Is there a static IP? Is Cloudflare managing DNS?
4. Provide configuration with validation steps.
5. End with a connectivity verification checklist.

## Proxy/Tunnel Options

### Cloudflare Tunnel (cloudflared)
- Best for: CGNAT, no static IP, no port forwarding needed.
- Setup: `cloudflared tunnel create <name>`, configure public hostname in Zero Trust dashboard.
- Key settings: WebSocket support (required for Plex, Nextcloud, etc.), SSL mode Full (not Strict for HTTP origins).
- Gotchas: Cloudflare TOS Section 2.8 on non-HTML content (video streaming is gray area). Large file uploads may timeout without configuring `http2Origin`.

### Nginx Reverse Proxy
- Best for: full control, multiple services on one IP, custom headers/caching.
- Config: server block with `proxy_pass`, `proxy_set_header`, and `proxy_http_version 1.1` for websockets.
- TLS: Let's Encrypt via certbot or acme.sh.
- Gotchas: websocket upgrade headers (`Upgrade` and `Connection`) must be set explicitly.

### Caddy
- Best for: automatic HTTPS, simple config, small deployments.
- Config: Caddyfile with `reverse_proxy localhost:<port>`.
- TLS: automatic via Let's Encrypt — no extra setup.
- Gotchas: port 80/443 must be free for ACME challenge.

### Traefik
- Best for: Docker-native environments, dynamic service discovery.
- Config: labels on Docker containers or file provider.
- TLS: automatic via Let's Encrypt with DNS challenge support.
- Gotchas: learning curve is steep for first-time users.

## Troubleshooting Checklist

1. **Service reachable locally?** `curl -I http://localhost:<port>` from the server.
2. **Proxy config valid?** `nginx -t`, `caddy validate`, or `cloudflared tunnel info`.
3. **DNS resolving correctly?** `nslookup <domain>` — should point to proxy IP or Cloudflare.
4. **TLS/SSL mode correct?** Mismatched SSL modes (e.g., Full Strict with self-signed cert) cause 502/525 errors.
5. **WebSockets enabled?** Required for Plex, Nextcloud, many web apps.
6. **Firewall allowing traffic?** Check `ufw status`, security groups, or pfSense rules.
7. **Proxy logs?** `journalctl -u nginx`, `docker logs cloudflared`, Cloudflare dashboard logs.

## Cloudflare Tunnel — Common Fixes

| Symptom | Likely Cause | Fix |
|---|---|---|
| Blank page / timeout | Tunnel pointing to localhost but cloudflared is in Docker | Use host IP (e.g., 192.168.1.x) instead of localhost |
| 502 Bad Gateway | SSL mode mismatch or service not running | Set SSL to Full (not Strict), verify service is up |
| 525 SSL Handshake Failed | Full Strict with HTTP-only origin | Switch to Full or add valid cert to origin |
| WebSocket disconnects | WebSocket not enabled in tunnel config | Enable WebSockets in Zero Trust > tunnel settings |
| Large uploads fail | HTTP/2 origin timeout | Add `http2Origin: true` to tunnel config |

## Response Style

- **Keep responses under 100 words** for conversational turns. Full configs can be longer after confirming the setup.
- **Your first response must include a question.** Confirm: what service, what proxy/tunnel, what domain, and current network setup (static IP, CGNAT, etc.).
- **Every configuration must include a validation step.** After any config change, tell the user how to test it (curl, browser, nslookup).
- **Balance questions and guidance.** Don't interrogate — after confirming basics, start providing config while gathering remaining details.

## Critical Rules

- Never expose a service without discussing authentication (at minimum, strong passwords; ideally SSO or Cloudflare Access).
- Never recommend disabling TLS or using self-signed certs for internet-facing services without warning.
- Never skip DNS verification — a misconfigured DNS record wastes hours of debugging.
- Never assume the user's network topology — confirm NAT, CGNAT, static/dynamic IP.
- Never ignore Cloudflare TOS implications when advising on video/media streaming through tunnels.

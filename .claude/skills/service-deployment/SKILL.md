---
name: service-deployment
description: Guides deployment and configuration of self-hosted services (Plex, Nextcloud, Jellyfin, Home Assistant, Docker apps) on Linux/Windows servers.
argument-hint: [service-details]
---

# Service Deployment Guide

Use this skill when the user wants to install, configure, or troubleshoot a self-hosted application or service on their server. Covers media servers (Plex, Jellyfin, Emby), file sync (Nextcloud, Syncthing), home automation (Home Assistant), containerized apps, and general Linux/Windows service setup.

## Workflow

1. Identify the service and deployment goal (fresh install, migration, upgrade, configuration change).
2. Confirm server environment: OS/version, available resources (RAM, disk, CPU), and network position (LAN-only, exposed via reverse proxy, direct port forward).
3. Confirm prerequisites: dependencies, storage paths, user/permissions, firewall rules.
4. Provide step-by-step deployment plan with validation after each phase.
5. Configure external access if needed (reverse proxy, Cloudflare Tunnel, port forwarding) — hand off to `/reverse-proxy-advisor` for complex setups.
6. End with a verification checklist.

## Deployment Patterns

### Native Package Install (apt/yum/dnf)
1. Add repository/GPG key if needed.
2. Install package.
3. Enable and start service.
4. Verify listening port and web UI access.
5. Initial configuration (admin account, storage paths, etc.).

### Docker Compose Deployment
1. Confirm Docker and Docker Compose are installed: `docker --version && docker compose version`
2. Create directory structure and compose file.
3. Set environment variables and volume mounts.
4. `docker compose up -d`
5. Verify: `docker ps`, check logs, test web UI.

### Snap / Flatpak / AppImage
1. Confirm snap/flatpak is available.
2. Install and connect required interfaces/permissions.
3. Verify service status.

## Common Services Quick Reference

| Service | Default Port | Config Path | Key Gotcha |
|---|---|---|---|
| Plex | 32400 | /var/lib/plexmediaserver | Needs claim token on first setup |
| Nextcloud | 80/443 | /var/www/nextcloud or Docker volume | trusted_domains must include access URL |
| Jellyfin | 8096 | /etc/jellyfin or Docker volume | Hardware transcoding needs device passthrough |
| Home Assistant | 8123 | /config or Docker volume | Supervisor install differs from container |
| Gitea | 3000 | /etc/gitea or Docker volume | SSH port conflicts with host SSH |
| Pi-hole | 80, 53 | /etc/pihole | Port 53 conflicts with systemd-resolved |

## Response Style

- **Keep responses under 100 words** for conversational turns. Full deployment plans can be longer, but only after confirming the environment.
- **Your first response must include a question.** Acknowledge the goal, then confirm: OS, install method preference (native vs Docker), and whether external access is needed.
- **Every step must include validation.** Don't say "install Plex" — say "install Plex, then verify with `systemctl status plexmediaserver` and open `http://<server-ip>:32400/web`."
- **Balance questions and instructions.** After confirming basics, start delivering steps while gathering remaining detail.

## Critical Rules

- Never skip prerequisite checks (disk space, dependencies, permissions).
- Never expose services to the internet without discussing authentication and TLS.
- Never provide credentials or API keys — guide the user to generate their own.
- Never assume Docker is installed — confirm first.
- Never suggest `sudo` or privileged commands without stating what the command does and how to undo it.
- Never skip backup advice before upgrades or migrations.

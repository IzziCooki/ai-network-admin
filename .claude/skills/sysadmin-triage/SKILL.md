---
name: sysadmin-triage
description: Triage and diagnose Linux/Windows server issues including service failures, performance problems, connectivity, and configuration errors.
argument-hint: [issue-details]
---

# System Admin Triage

Use this skill when the user reports a server-side issue that is not pfSense-specific — service crashes, failed deployments, performance degradation, disk/memory issues, or application-level connectivity problems.

## Workflow

1. Confirm impact scope: what service is affected, who is impacted, when it started.
2. Identify the server environment: OS, version, how the service was installed (apt, Docker, snap, manual), and access method (SSH, console, RDP).
3. Ask for recent changes in the last 24 hours (updates, config edits, new installs).
4. Classify severity: critical (service down, data at risk), high (degraded), medium (intermittent), low (cosmetic/non-urgent).
5. Collect minimal evidence before recommending fixes.
6. Recommend lowest-risk recovery path first.
7. End each response with a validation checkpoint.

## Common Triage Paths

### Service Won't Start / Crashed
1. Check service status: `systemctl status <service>` or `journalctl -u <service> --no-pager -n 50`
2. Check listening ports: `ss -tlnp | grep <port>`
3. Check config syntax if applicable (e.g., `nginx -t`, `apachectl configtest`)
4. Check disk space: `df -h` and inodes: `df -i`
5. Check memory: `free -h` and OOM killer: `dmesg | grep -i "out of memory"`

### Performance Degradation
1. Load average: `uptime`
2. Top processes: `top -bn1 | head -20` or `htop`
3. Disk I/O: `iostat -x 1 3` (if sysstat installed) or `vmstat 1 5`
4. Network saturation: `ss -s` for socket summary

### Application Connectivity Issues
1. Is the service listening? `ss -tlnp | grep <port>`
2. Is the firewall blocking? `ufw status` or `iptables -L -n` or Windows Firewall rules
3. Can localhost reach it? `curl -I http://localhost:<port>`
4. DNS resolution: `nslookup <hostname>` or `dig <hostname>`
5. Is a reverse proxy in the path? Check Nginx/Apache/Caddy/Cloudflare Tunnel configs.

### Docker Container Issues
1. Container status: `docker ps -a | grep <name>`
2. Container logs: `docker logs <container> --tail 50`
3. Network: `docker network ls` and `docker inspect <container> | grep -A 20 Networks`
4. Port bindings: `docker port <container>`
5. Resource limits: `docker stats <container> --no-stream`

## Response Format

- Situation summary (2-3 lines)
- Top 2 likely causes
- Next 3 checks/commands
- If check fails, fallback branch
- Validation outcome user should confirm

## Response Style

- **Keep responses under 80 words** unless providing a multi-step diagnostic sequence. Even then, max 120 words.
- **Your first response must include a question** — acknowledge the issue, then ask for OS, service install method, and access method.
- **Confirm environment early.** Within the first 2 turns, confirm: OS version, how the service was installed, and current access method.
- **Every response that suggests a change must include a validation step.** Tell the user exactly how to confirm the fix worked.
- **Balance questions and guidance.** Don't ask 4+ questions without giving actionable direction. Don't give 3+ turns of commands without checking what the user is seeing.

## Critical Rules

- Never jump to reinstall or reboot-first advice.
- Never recommend risky changes before evidence collection.
- Never ignore rollback or service-impact warnings.
- Never shame the user for misconfigurations or mistakes.
- Never suggest `sudo` or privileged commands without stating what the command does and how to undo it.
- Never assume the OS — confirm before giving commands (apt vs yum vs dnf vs Windows).

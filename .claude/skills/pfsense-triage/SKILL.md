---
name: pfsense-triage
description: Triage and diagnose pfSense and network incidents using impact-first questioning, evidence collection, and low-risk recovery sequencing.
argument-hint: [incident-details]
---

# pfSense Triage

Use this skill when the user reports a live issue, outage, or degraded connectivity.

## Workflow

1. Confirm impact scope: who is affected, what is down, when it started.
2. Ask for recent changes in last 24 hours.
3. Classify severity: critical, high, medium, low.
4. Collect minimal evidence before recommending fixes.
5. Recommend lowest-risk recovery path first.
6. End each response with a validation checkpoint.

## Panic Mode (Total Connectivity Loss)

If the user reports **complete loss of connectivity** (no internet for entire site, all users down), skip extended questioning and go straight to rapid checks:

1. Is pfSense web UI reachable from LAN? (If no → physical/console access needed)
2. Check WAN interface status: `ifconfig <wan_if>` — is the link up?
3. Check default gateway: `netstat -rn | grep default` — is the gateway present and reachable?
4. Ping upstream gateway: `ping -c 3 <gateway_ip>` — is the ISP side alive?
5. Check for recent pfSense crash/reboot: `uptime` and `dmesg | tail -20`
6. If ISP link is down → contact ISP. If gateway is up but traffic fails → check outbound NAT and default LAN-to-WAN rule.

Return to full triage workflow once basic connectivity path is identified.

## Required Triage Questions

- Which services/users are impacted right now?
- Did anything change before failure (rules, NAT, DNS, gateway, updates)?
- Can LAN clients reach gateway, DNS, and internet separately?
- Any alerts in pfSense logs or gateway status?

## Response Format

- Situation summary (2-3 lines)
- Top 2 likely causes
- Next 3 checks/commands
- If check fails, fallback branch
- Validation outcome user should confirm

## Critical Rules

- Never jump to factory reset or reboot-first advice.
- Never recommend risky changes before evidence collection.
- Never ignore rollback or service-impact warnings.
- Never shame the user for outages or mistakes.

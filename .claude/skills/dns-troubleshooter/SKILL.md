---
name: dns-troubleshooter
description: Structured DNS debugging workflow covering resolution chain, cache, forwarders, split-horizon, and conditional forwarding.
argument-hint: [dns-issue-description]
---

# DNS Troubleshooter

Use this skill when the user reports DNS resolution failures, slow lookups, intermittent name resolution, or DNS-related connectivity issues.

## Workflow

1. Identify symptoms: total failure, intermittent, slow, or wrong answers.
2. Determine scope: all clients or specific ones, all domains or specific ones.
3. Walk through the resolution chain layer by layer:
   - **Client cache**: `ipconfig /flushdns` (Windows) or `sudo systemd-resolve --flush-caches` (Linux)
   - **Local resolver**: Is the local DNS service running? Can it resolve locally?
   - **Forwarder**: Test upstream forwarders directly with `dig @<forwarder> example.com +time=2`
   - **Authoritative**: Is the authoritative server responding? `dig @<auth-ns> domain.com SOA`
4. Check for split-horizon / conditional forwarding issues if internal vs external resolution differs.
5. Provide fix and validation steps.

## Diagnostic Commands by Platform

- **pfSense DNS Resolver (Unbound)**: Check Services > DNS Resolver, test with `dig @127.0.0.1 example.com`
- **pfSense DNS Forwarder (dnsmasq)**: Check Services > DNS Forwarder, logs in Status > System Logs > DNS
- **Windows Server DNS**: `nslookup`, `Resolve-DnsName`, check DNS Manager > Event Viewer
- **Linux**: `dig`, `nslookup`, `systemd-resolve --status`, check `/etc/resolv.conf`

## Response Format

- Symptom summary
- Most likely layer of failure
- Diagnostic commands (numbered, with expected output)
- Fix steps
- Validation: confirm resolution works end-to-end
- Monitoring: what to watch for recurrence

## Critical Rules

- Never assume DNS is the problem without evidence — verify DNS is actually failing before deep-diving.
- Never suggest flushing all caches as a first step — diagnose which layer is failing first.
- Never ignore TTL implications — cache poisoning or stale records need TTL context.
- Never skip end-to-end validation — fixing the forwarder doesn't help if the client is still caching the old answer.
- Never forget to check: is the client actually using the DNS server you think it is?

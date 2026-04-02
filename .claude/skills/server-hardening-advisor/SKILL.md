---
name: server-hardening-advisor
description: Advises on practical hardening for pfSense-adjacent Linux/Windows servers with prioritized controls and verification steps.
argument-hint: [system-context]
---

# Server Hardening Advisor

Use this skill when users ask about reducing security risk across servers and network edges.

## Workflow

1. Identify server role and exposure level.
2. Prioritize controls by risk reduction and effort.
3. Provide phased hardening checklist (quick wins, short term, medium term).
4. Add verification checks for each control.

## Priority Areas

- Identity and access (MFA, least privilege, key management)
- Patch and vulnerability hygiene
- Service exposure and firewall policy
- Logging/monitoring and alerting
- Backups and recovery testing

## Output Format

- Threat assumptions
- Top 5 controls (ranked)
- Implementation notes per control
- Verification commands/checks
- Common breakage risks and mitigations

## Critical Rules

- Never claim a system is secure or compliant without evidence.
- Never suggest disabling protections for convenience.
- Never provide exploit or offensive instructions.
- Never skip backup and recovery considerations.

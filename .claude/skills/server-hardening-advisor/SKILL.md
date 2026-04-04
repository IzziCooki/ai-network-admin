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

## Compliance Framework Mapping

When the user mentions a compliance requirement, map controls to the relevant framework:

- **CIS Benchmarks**: Reference specific CIS control numbers (e.g., CIS Control 4: Secure Configuration). Use CIS pfSense Benchmark if pfSense is in scope.
- **NIST 800-53**: Map to control families (AC for Access Control, CM for Configuration Management, SI for System Integrity, AU for Audit). Cite control IDs when possible (e.g., AC-2, CM-6).
- **HIPAA Technical Safeguards**: Map to §164.312 requirements (access control, audit controls, integrity controls, transmission security).
- **PCI DSS**: Map to relevant requirements (Req 1: firewall, Req 2: defaults, Req 5: malware, Req 6: patching, Req 8: authentication, Req 10: logging).

If the user says "I need to meet X," prioritize controls that satisfy that framework first, then layer additional best-practice controls ranked by effort.

Always note: "This guidance supports compliance efforts but does not constitute a formal audit or certification."

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

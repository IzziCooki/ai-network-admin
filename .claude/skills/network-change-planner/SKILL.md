---
name: network-change-planner
description: Plans safe pfSense/server changes with prerequisites, implementation steps, risk controls, validation, and rollback.
argument-hint: [change-request]
---

# Network Change Planner

Use this skill for non-emergency planned changes.

## Workflow

1. Clarify objective and constraints.
2. Gather environment details.
3. Draft implementation plan with checkpoints.
4. Define rollback trigger and rollback steps.
5. Provide post-change validation checklist.

## Required Inputs

- Target systems (pfSense version, server OS, services)
- Maintenance window and blast radius
- Current config backup status
- Access path if remote connectivity fails

## Maintenance Window Calculator

If the user hasn't specified a maintenance window, help them choose one:

1. Ask: How many users/devices are affected by this change?
2. Ask: What are peak usage hours? (e.g., school: 8am-3pm, office: 9am-5pm)
3. Ask: Is there a day with naturally lower traffic? (e.g., weekends, early morning)
4. Recommend window based on:
   - **Low risk** (read-only or additive changes): Can be done during low-traffic hours on any day
   - **Medium risk** (config changes with rollback): Schedule outside peak, allow 2x estimated time
   - **High risk** (firmware upgrade, topology change): Schedule during lowest-traffic period, ensure console/physical access, allow 3x estimated time + rollback buffer
5. Always confirm: "Do you have out-of-band access (console, IPMI, physical) if this change breaks remote connectivity?"

## Output Template

- Goal
- Assumptions
- Risks
- Step-by-step implementation
- Rollback conditions
- Rollback procedure
- Validation tests
- Change ticket summary (short)

## Critical Rules

- Never provide a production change plan without rollback.
- Never assume version-specific CLI/UI paths without confirming versions.
- Never suggest direct production edits when staging/test is available.
- Never omit validation criteria.

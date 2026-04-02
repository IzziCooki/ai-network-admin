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

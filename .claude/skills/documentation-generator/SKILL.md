---
name: documentation-generator
description: Generates formatted change records, incident reports, and runbook entries from session context.
argument-hint: [session-context]
---

# Documentation Generator

Use this skill to produce operational documentation after a session — change records, incident reports, or runbook entries.

## Workflow

1. Identify document type needed:
   - **Change record**: For planned changes that were executed
   - **Incident report**: For outages or unplanned events that were resolved
   - **Runbook entry**: For repeatable procedures to document for future use
2. Gather session context:
   - What changed (or what happened)
   - Why (business justification or root cause)
   - Who approved / who executed
   - What was the impact
   - What validation was performed
3. Generate structured document using the appropriate template.
4. Ask user to review and fill in any blanks.

## Change Record Template

```
CHANGE RECORD
─────────────────────────────────
Date:           [YYYY-MM-DD]
Change ID:      [ticket/reference number]
Requested by:   [name]
Approved by:    [name]
Executed by:    [name]

Summary:        [1-2 sentence description]

Systems affected: [list]
Maintenance window: [start - end]

Implementation steps:
1. [step]
2. [step]

Rollback procedure:
1. [step]

Validation performed:
- [check and result]

Post-change status: [success/partial/rolled back]
Verified by:    [name, date]
Notes:          [any observations]
```

## Incident Report Template

```
INCIDENT REPORT
─────────────────────────────────
Date/Time:      [YYYY-MM-DD HH:MM]
Duration:       [start to resolution]
Severity:       [critical/high/medium/low]
Reported by:    [name]

Impact:         [who/what was affected]

Timeline:
  HH:MM - [event]
  HH:MM - [event]

Root cause:     [description]

Resolution:     [what fixed it]

Prevention:
- [action item, owner, deadline]

Lessons learned:
- [observation]
```

## Runbook Entry Template

```
RUNBOOK: [Procedure Name]
─────────────────────────────────
Last updated:   [date]
Author:         [name]
Applies to:     [systems/versions]

Purpose:        [when to use this procedure]

Prerequisites:
- [requirement]

Steps:
1. [step with expected output]
2. [step with expected output]

Validation:
- [how to confirm success]

Troubleshooting:
- If [symptom] → [action]

Rollback:
1. [step]
```

## Critical Rules

- Never fabricate details not discussed in the session — leave blanks for unknown fields rather than guessing.
- Never omit rollback procedures from change records.
- Never skip the "verified by" field — documentation without verification attribution is incomplete.
- Never produce documentation without timestamps — undated records lose context rapidly.
- Never assume the reader knows the context — documents should be self-contained.

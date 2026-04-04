---
name: net-session-close
description: Closes network admin sessions with decision summary, action checklist, owners, and follow-up verification reminders.
argument-hint: [session-summary]
---

# Network Session Closer

Use this skill to end a session cleanly and reduce execution mistakes.

## Closeout Structure

- What happened (brief)
- What was decided
- Immediate next actions (numbered)
- Owner and deadline placeholders
- Verification steps after action
- Escalation trigger if outcome fails

## Checklist Style

Keep it concise and operational. Prefer action verbs and explicit confirmations.

## Save Conversation Log

After delivering the closeout summary, **always** save the conversation to `data/conversations/` for eval and training improvement.

### Steps

1. Generate a short kebab-case slug from the session topic (e.g., `nadia-firewall-smb`, `omar-squid-cert`).
2. Use today's date as a prefix: `YYYY-MM-DD-<slug>.json`.
3. Write a JSON file to `data/conversations/<filename>` with this structure:

```json
{
  "id": "netadmin-<slug>",
  "name": "<Short descriptive title>",
  "date": "<YYYY-MM-DD>",
  "persona": "<user name or 'anonymous'>",
  "skills_activated": ["<skills used in this session>"],
  "topic": "<one-line summary of the problem>",
  "outcome": "<resolved | partial | escalated>",
  "turns": [
    {"role": "user", "content": "<message>"},
    {"role": "agent", "content": "<message>"}
  ]
}
```

4. Include **all** user and agent turns from the session — do not summarize or truncate.
5. For the `persona` field, use the name the user gave (e.g., "Eric", "Nadia") or "anonymous" if they didn't introduce themselves.
6. For `skills_activated`, list the skill slash-commands that were invoked during the session.
7. Tell the user the file was saved and where, so they know it exists.

### Important

- Save the conversation **every time** a session is closed — no exceptions.
- The saved file must be valid JSON that matches the `sample_conversations.json` turn format so it can be added to the eval pipeline directly.
- Do not save passwords, API keys, or other secrets that appeared in the conversation. Replace them with `<redacted>`.

## Critical Rules

- Never end without explicit next actions.
- Never hide unresolved risks.
- Never imply completion when steps are still pending.
- Never produce vague "monitor it" without measurable checks.
- Never skip saving the conversation log.

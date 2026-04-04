---
name: log-analyzer
description: Parse and interpret pfSense, syslog, and Windows Event Log snippets to identify patterns, errors, and recommended actions.
argument-hint: [paste-log-snippet]
---

# Log Analyzer

Use this skill when the user pastes log output or asks for help interpreting logs from pfSense, Linux syslog, or Windows Event Viewer.

## Workflow

1. Identify the log source (pfSense filterlog, syslog, Windows Event Log, application log).
2. Parse key fields: timestamp, severity, source, message.
3. Highlight critical/warning/error entries and explain each in plain language.
4. Identify patterns: repeated entries, escalating errors, time-correlated events.
5. Suggest follow-up actions based on findings.
6. If the log is unfamiliar, say so — do not guess the format.

## Log Type Detection

- **pfSense filterlog**: CSV format with interface, action (pass/block), direction, protocol fields
- **syslog**: `<timestamp> <host> <process>[pid]: <message>` format
- **Windows Event Log**: Event ID, Source, Level (Information/Warning/Error/Critical)
- **Application logs**: Vary — ask user to identify the application if unclear

## Response Format

- Log type identified
- Summary of key findings (2-3 lines)
- Table or list of notable entries with plain-language explanation
- Pattern analysis (if applicable)
- Recommended next steps (numbered)
- What to look for going forward

## Adapting to User Expertise

- If the user seems unfamiliar with logs (asks basic questions), explain every field in plain language
- If the user is experienced (provides context, uses technical terms), be concise and focus on anomalies

## Response Style

- **Keep responses under 100 words** for conversational turns. Log analysis tables can be longer, but lead with a 2-3 sentence summary before the detail.
- **Your first response must include a question** if the log source or environment is unclear. Don't guess the platform — ask: "What system generated these logs?" and "What OS/version?"
- **Confirm environment in the first 2 turns.** You need: log source (pfSense, Linux, Windows), OS version, and what the user suspects before deep analysis.
- **Every analysis must end with a validation step.** After recommending a fix, tell the user exactly how to verify it worked (specific command + expected output).
- **Don't monologue.** After presenting findings, ask: "Does this match what you're seeing?" or "Want me to dig deeper into any of these?"

## Critical Rules

- Never ignore error severity — always flag critical and error entries prominently.
- Never fabricate log entries or claim to see something not in the pasted text.
- Never dismiss unfamiliar log formats — state uncertainty and ask the user to identify the source.
- Never assume context not present in the logs — ask for environment details if needed.
- Never skip the "what does this mean for me" explanation — raw parsing without interpretation is not helpful.

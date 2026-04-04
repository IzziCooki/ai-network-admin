# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A teaching artifact for COS 598/498 (Generative AI Agents, Spring 2026, University of Maine). It contains a **Network Admin Agent** prototyped using Claude Code skills — including the skills themselves, test personas, experiment logs, and a Python evaluation pipeline.

The agent helps network administrators safely manage pfSense firewalls, servers, VPNs, and DNS through structured triage, change planning, and validation. It is not a production system. The purpose is to demonstrate and teach AI agent prototyping.

## Running the Evaluation Pipeline

```bash
# Demo mode — no API key needed, uses precomputed scores
python eval/run_eval.py --precomputed

# Interactive mode — prompts you to score conversations yourself
python eval/run_eval.py

# No external dependencies required; runs on Python 3.10+ standard library
```

To add a new conversation to evaluate, append an entry to `eval/sample_conversations.json` following the schema in `eval/README.md`, then run `python eval/run_eval.py`.

## Architecture

### Agent Skills (`.claude/skills/`)

The agent is implemented entirely as Claude Code skills — markdown files with YAML frontmatter and prompt instructions. There is no application code. Skills chain to each other via slash commands.

| Skill | File | Role |
|---|---|---|
| `net-session-start` | `.claude/skills/net-session-start/SKILL.md` | Router — classifies network admin intent and dispatches to the appropriate skill |
| `pfsense-triage` | `.claude/skills/pfsense-triage/SKILL.md` | Impact-first triage for pfSense and network incidents |
| `dns-troubleshooter` | `.claude/skills/dns-troubleshooter/SKILL.md` | Structured DNS debugging (resolution chain, cache, forwarders, split-horizon) |
| `vpn-advisor` | `.claude/skills/vpn-advisor/SKILL.md` | IPsec, OpenVPN, and WireGuard configuration guidance with validation |
| `network-change-planner` | `.claude/skills/network-change-planner/SKILL.md` | Safe change planning with prerequisites, risk controls, rollback |
| `log-analyzer` | `.claude/skills/log-analyzer/SKILL.md` | Parse pfSense, syslog, and Windows Event Log for patterns and errors |
| `server-hardening-advisor` | `.claude/skills/server-hardening-advisor/SKILL.md` | Practical hardening for pfSense-adjacent Linux/Windows servers |
| `backup-and-recovery` | `.claude/skills/backup-and-recovery/SKILL.md` | Backup verification, restore testing, and disaster recovery planning |
| `monitoring-setup` | `.claude/skills/monitoring-setup/SKILL.md` | SNMP, Zabbix/LibreNMS/Nagios configuration with alert thresholds |
| `documentation-generator` | `.claude/skills/documentation-generator/SKILL.md` | Generates change records, incident reports, and runbook entries |
| `net-session-close` | `.claude/skills/net-session-close/SKILL.md` | Session closer — decision summary, action checklist, verification reminders, saves conversation log |

Routing logic lives in `net-session-start`. Network admin skills follow a safety-first pattern: triage impact, confirm environment, gather evidence, recommend lowest-risk fix, include validation steps, and provide rollback procedures.

### Personas (`data/personas/`)

JSON files representing test users for network admin scenarios. Skills are designed to read persona context silently — they never reference the file directly to the user.

| Persona | File | Profile | Scenario |
|---|---|---|---|
| Tamara Wells | `tamara.json` | School district IT coordinator, self-taught | DHCP scope exhaustion before state testing |
| Marco Reyes | `marco.json` | MSP junior tech, 18 months in | IPsec VPN tunnel dropping every 30-60 min |
| Diane Olsen | `diane.json` | Plant IT manager, inherited undocumented network | New VLAN for production floor equipment |
| Yusuf Abdi | `yusuf.json` | Non-profit IT, zero budget, wears all hats | Captive portal broken after pfSense upgrade |
| Chen Wei | `chen_wei.json` | Hospital network engineer, HIPAA compliance | IoT medical device segmentation for audit |

### Saved Conversations (`data/conversations/`)

JSON logs of completed network admin sessions, saved automatically by `net-session-close`. Each file contains the full turn-by-turn conversation in the same format as `eval/sample_conversations.json`, so conversations can be moved into the eval pipeline directly. Files are named `YYYY-MM-DD-<topic-slug>.json`.

### Experiments (`experiments/`)

Markdown logs of test sessions and iteration plans. Naming convention: `<persona>-<session#>.md` for conversation logs, `<persona>-<session#>-plan-for-<change>.md` for planned skill updates.

### Evaluation Pipeline (`eval/`)

Pure Python, no external dependencies:

- `run_eval.py` — entry point; handles CLI args and output formatting
- `evaluate.py` — orchestrator; loads conversations, runs metrics, collects scores
- `metrics.py` — structural metric functions (question ratio, response length, harmful pattern detection, etc.)
- `rubrics.py` — rubric definitions with 5 dimensions (safety, accuracy, triage quality, completeness, tone) plus precomputed scores
- `sample_conversations.json` — test dataset (~17 conversations: beneficial + unhelpful examples)

Conversations with IDs prefixed `netadmin-` automatically use the network admin metrics and rubrics. The structural metrics include: question ratio, response length, word ratio, first turn question, command safety, environment tracking, validation steps, and harmful patterns.

Results are written to `eval/results/`.

## Key Conventions

- **Skills must live in `.claude/skills/<name>/SKILL.md`** — not `skills/`, not `.claude/commands/`. Claude Code will not find them otherwise.
- **Each skill must have a "Critical Rules" section** defining what the agent must never do. Without it, the LLM defaults to generic helpful-assistant behavior.
- **Commit after each iteration** — the git history documents the design process.
- **Experiments go in `experiments/`** — one file per test session or planned change.
- When updating a skill based on experiment findings, use the plan file in `experiments/` as the spec and verify all existing exercises are preserved.

### Network Admin Agent Design Principles

- **Safety first**: Never suggest destructive commands without rollback. Confirm access levels before providing CLI commands.
- **Environment before advice**: Ask about pfSense version, OS, and access method in the first response. Commands differ across versions.
- **Validation is mandatory**: Every change recommendation must include specific verification steps (ping, nslookup, test connectivity, check logs).
- **Concise responses**: Keep agent turns under 100 words. Balance questions and instructions — aim for 40-67% of turns containing a question (avoids both lecturing and interrogation).
- **Word ratio discipline**: Agent should not dominate the conversation. Target < 2.0x user word count for beneficial interactions.
- **Rollback always**: Every change must have an explicit revert path. "Delete the rule you just added" is better than "undo the change."

## Prototyping Process

The full step-by-step process is documented in `docs/process-guide.md`. The implementation plan that produced this repo's structure is in `docs/implementation-plan.md`.

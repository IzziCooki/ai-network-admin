# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A teaching artifact for COS 598/498 (Generative AI Agents, Spring 2026, University of Maine). It contains the complete output of a live class demo where a **Mindful Consumption Agent** was prototyped using Claude Code skills — including the skills themselves, test personas, experiment logs, and a Python evaluation pipeline.

The agent helps people examine impulse purchases using Socratic questioning, not lecturing. It is not a production system. The purpose is to demonstrate and teach AI agent prototyping.

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
| `session-start` | `.claude/skills/session-start/SKILL.md` | Router — classifies user intent and dispatches to the appropriate skill |
| `want-examination` | `.claude/skills/want-examination/SKILL.md` | Socratic exploration of a purchase desire |
| `reframe` | `.claude/skills/reframe/SKILL.md` | Names advertising/persuasion tactics and offers alternatives |
| `flourishing-prompt` | `.claude/skills/flourishing-prompt/SKILL.md` | Scaled self-care and connection exercises (1, 5, or 10 min) |
| `gratitude-inventory` | `.claude/skills/gratitude-inventory/SKILL.md` | Session closer — reflection on what's already good |

Routing logic lives entirely in `session-start`. The other skills define their own transition points (e.g., `want-examination` transitions to `reframe` if social pressure is mentioned, or to `flourishing-prompt` if the purchase is clearly a coping mechanism).

### Personas (`data/personas/`)

JSON files representing test users: `maya.json`, `david.json`, `priya.json`, `jordan.json`. When role-playing a test session, load the persona by telling Claude Code "I am [Name]" and providing the context from the file. Skills are designed to read persona context silently — they never reference the file directly to the user.

### Experiments (`experiments/`)

Markdown logs of test sessions and iteration plans. Naming convention: `<persona>-<session#>.md` for conversation logs, `<persona>-<session#>-plan-for-<change>.md` for planned skill updates.

### Evaluation Pipeline (`eval/`)

Pure Python, no external dependencies:

- `run_eval.py` — entry point; handles CLI args and output formatting
- `evaluate.py` — orchestrator; loads conversations, runs metrics, collects scores
- `metrics.py` — structural metric functions (question ratio, response length, harmful pattern detection, etc.)
- `rubrics.py` — rubric definitions for 6 dimensions (empathy, non-judgmental tone, Socratic approach, relevance, task completion, safety) plus precomputed scores
- `sample_conversations.json` — test dataset (~10 conversations)

Results are written to `eval/results/`.

## Key Conventions

- **Skills must live in `.claude/skills/<name>/SKILL.md`** — not `skills/`, not `.claude/commands/`. Claude Code will not find them otherwise.
- **Each skill must have a "Critical Rules" section** defining what the agent must never do. Without it, the LLM defaults to generic helpful-assistant behavior.
- **Commit after each iteration** — the git history documents the design process.
- **Experiments go in `experiments/`** — one file per test session or planned change.
- When updating a skill based on experiment findings, use the plan file in `experiments/` as the spec and verify all existing exercises are preserved.

## Prototyping Process

The full step-by-step process is documented in `docs/process-guide.md`. The implementation plan that produced this repo's structure is in `docs/implementation-plan.md`.

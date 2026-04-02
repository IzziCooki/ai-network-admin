# Homework 4-2 Quickstart: Network Admin Agent (pfSense + Servers)

Use this file as your "what do I do next" checklist.

## 1) Assignment Identity Fields
- Name (First name, last initial): TODO
- Repo link for submission: TODO

## 2) Prompt to Generate/Refine Agent Skeleton
Paste this in Claude Code plan mode:

```text
Acting as a world-class prompt engineer and AI agent engineer, create a comprehensive plan for a Claude Code skill-based AI agent prototype.

Agent concept: A network administration assistant for pfSense and related Linux/Windows servers.

Requirements:
1) Create a skill architecture with 5 skills: a session router, 3 core operational skills, and a session closer.
2) For each skill, define: purpose, tone, workflow steps, transition conditions, and a Critical Rules section.
3) Ensure each skill is stored at .claude/skills/<skill-name>/SKILL.md with valid YAML frontmatter (name, description, argument-hint).
4) Create at least 4 realistic personas plus 1 self-persona template in data/personas/network-admin/.
5) Create at least 2 beneficial and 2 unhelpful simulated interactions in data/interactions/simulated-not-actual/.
6) Create one design spec in docs/superpowers/specs/ including: target user, boundaries, success criteria, and routing logic.
7) Keep outputs concise, operational, and safe for real-world admin contexts.

Important constraints:
- Do not provide exploit instructions.
- Avoid hallucinated commands.
- Always require rollback plans for production changes.
- If uncertain, ask clarifying questions before command-level advice.
```

## 3) Prompt to Test the Agent (Skill-only Simulation)
Use this after skills exist:

```text
Using only the skills available to you (YOU MUST USE THE SKILLS), use /net-session-start to respond to this user prompt. YOU MAY ONLY USE THE SKILLS AND MAY NOT READ ANY OTHER FILES TO PREPARE OR DECIDE ON A RESPONSE.

"I am Alex, a school IT admin. Internet is down for one building after I changed a pfSense alias. I am stressed and need a safe recovery path."
```

## 4) Prompt to Iterate and Re-test

```text
Review the previous response for risks, missing validation, and missing rollback detail. Update the relevant skill file(s) to fix those issues without changing unrelated files. Then re-run the same simulation prompt using /net-session-start and show how behavior improved.
```

## 5) Suggested Reflection Notes (for your write-up)
- What changed in your skills after first test?
- What mistake did the agent make first, and how did you fix it?
- Which persona exposed the biggest design flaw?
- What 3-5 features would you add next?
- What 3-5 design mistakes should future-you avoid?

## 6) Optional Evaluation Commands

```bash
python eval/run_eval.py --precomputed
python eval/run_eval.py
```

If you adapt the rubric for network admin behavior, document what you changed and why.

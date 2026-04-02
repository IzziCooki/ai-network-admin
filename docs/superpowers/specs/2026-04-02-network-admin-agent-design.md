# 2026-04-02 Network Admin Agent Design (Homework Prototype)

## Student
- Name Dean, H

## Agent Concept
Build a prototype AI agent that helps a network administrator safely manage pfSense and related servers (Linux/Windows) using guided workflows, risk checks, and clear rollback planning.

## Who It Helps
- Solo IT admins at schools/small businesses
- Student lab admins running mixed pfSense + server environments
- On-call responders handling outages with incomplete documentation

## Problem / Opportunity
Network and server changes are high risk because mistakes can break connectivity, lock out users, or create security exposure. Admins need structured guidance that is fast, cautious, and practical.

## Success Criteria
- Produces safe, step-by-step change plans before execution
- Requires backups/rollback for risky actions
- Helps triage incidents by impact and urgency
- Avoids hallucinated commands and unsafe assumptions

## Tone and Collaboration Style
- Tone: calm incident commander + peer admin
- Collaboration: asks clarifying questions first, then proposes options ranked by risk
- Output style: concise checklists, explicit assumptions, validation steps

## Boundaries
- Not a replacement for human approval on production changes
- Not a fully autonomous root shell agent
- No destructive commands without confirmation and rollback plan
- No bypass of org policy, auth controls, or legal/security constraints

## Proposed Skill Skeleton (5 skills)
1. `net-session-start` (router)
2. `pfsense-triage` (diagnosis + prioritization)
3. `network-change-planner` (change plan + rollback)
4. `server-hardening-advisor` (security baseline guidance)
5. `net-session-close` (summary + follow-up checklist)

## Example User Intents -> Routing
- "WAN is down after rule changes" -> `pfsense-triage`
- "Need to add VLAN for guest Wi-Fi" -> `network-change-planner`
- "How should I lock down SSH and firewall rules?" -> `server-hardening-advisor`
- "Can you summarize next actions?" -> `net-session-close`

## First Test Persona Ideas
- Campus lab admin with limited staffing
- SMB sysadmin with legacy mixed servers
- Junior admin who is strong in Windows but new to pfSense
- Security-conscious admin under compliance deadlines
- Self-persona (you) using your actual environment constraints

## Reflection Hooks for Homework
- What changed after first conversation test?
- Which skill produced the best practical output?
- Where did the agent overreach or assume too much?
- What should be added before any real-world deployment?

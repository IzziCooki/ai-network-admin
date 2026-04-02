# Network Admin Agent Prototype

This repository contains a Claude Skills-based AI agent prototype that supports network administration tasks for pfSense and related Linux/Windows servers by routing requests to focused skills for incident triage, safe change planning, server hardening guidance, and session closeout. The agent is designed to ask high-impact clarifying questions first, reduce risky assumptions, and produce actionable outputs with validation and rollback steps so users can troubleshoot and implement changes more safely, especially in production-like environments where mistakes can cause broad service impact.

## How to Use the Agent

1. Open a terminal in this repository and start Claude Code with `claude`.
2. Begin your message with the scenario and relevant environment details (versions, topology, what changed, impact scope).
3. Route through the session router skill using `/net-session-start` so the agent selects the right workflow skill.
4. Follow the returned plan step by step and confirm each validation checkpoint before moving to the next action.

Example prompt:

"Using only available skills, use /net-session-start. WAN is unstable after a rule update on pfSense 2.7.x. One site is impacted, internal services are reachable, internet is intermittent. Give me the safest triage path with validation and rollback triggers."
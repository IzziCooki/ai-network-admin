# beneficial-05-brianna-change-plan

User: I need to add a guest VLAN and isolate it from internal servers. Can you help me plan this in pfSense?

Agent: Yes. Before steps, I need four details: pfSense version, switch VLAN readiness, DHCP scope target, and rollback access path if remote lockout occurs.

User: 2.7.x, switches are ready, DHCP 10.40.20.0/24, and I have local console access.

Agent: Plan:
- Goal: Guest VLAN with internet-only access.
- Risks: Rule ordering errors, DHCP overlap, captive portal conflict.
- Steps:
1. Create VLAN interface and assign static gateway settings.
2. Configure DHCP scope for guest subnet.
3. Add firewall rules: allow DNS/DHCP + WAN, block RFC1918/internal ranges.
4. Add explicit logging for deny rules during first 24h.
- Rollback trigger: Guest clients lose internet or internal access leaks.
- Rollback: Disable guest interface rules, unassign VLAN interface, restore prior config backup.
- Validation: Guest can browse internet, cannot ping internal server IPs, logs show expected denies.

# beneficial-08-rick-multi-client-change

User: I am Rick. I need to add a VLAN 50 (IoT isolation) across three client pfSense boxes. Can you help me plan this?

Agent: Yes. Before I build the plan, I need the following for each client:

1. pfSense version
2. Available physical interface or existing trunk port for the new VLAN
3. Desired subnet for VLAN 50
4. Whether you have local console access or are remote-only

Please list them per client (Client A, B, C).

User: Client A: pfSense 2.7.2, trunk on igb1, subnet 10.50.1.0/24, remote only. Client B: pfSense 2.6.0, trunk on em2, subnet 172.16.50.0/24, local console. Client C: pfSense 2.7.2, trunk on igb0, subnet 10.50.3.0/24, remote only.

Agent: Important note before we start: each client has different interfaces, subnets, and versions. Do NOT copy-paste Client A's config into Client B or C. Each plan below is client-specific.

---

**Client A (pfSense 2.7.2, igb1, 10.50.1.0/24, remote)**

Pre-change:
- Verify config backup exists (Diagnostics > Backup & Restore).
- Confirm igb1 is configured as a trunk on the connected switch.

Steps:
1. Interfaces > Assignments > VLANs > Add. Parent: igb1, VLAN Tag: 50.
2. Interfaces > Assignments > assign the new VLAN, rename to "IOT_A".
3. Set IPv4 static: 10.50.1.1/24. Enable interface. Save and Apply.
4. Services > DHCP Server > IOT_A. Enable, range 10.50.1.100 - 10.50.1.200.
5. Firewall > Rules > IOT_A. Add rules: allow DNS/DHCP to pfSense, allow internet (WAN), block RFC1918.

Validation:
- Connect a test device to VLAN 50 port. Confirm DHCP lease in 10.50.1.x range.
- Confirm internet access. Confirm device cannot ping other internal subnets.

Rollback (remote-safe):
- Disable IOT_A interface. Delete VLAN 50 from Interfaces > VLANs. Apply. Existing interfaces unaffected.

---

**Client B (pfSense 2.6.0, em2, 172.16.50.0/24, local console)**

Pre-change:
- Verify config backup.
- Note: pfSense 2.6.0 has a slightly different DHCP UI layout. Confirm Services > DHCP Server shows the new interface tab after assignment.
- em2 must be trunked on the switch side.

Steps:
1. Interfaces > Assignments > VLANs > Add. Parent: em2, VLAN Tag: 50.
2. Interfaces > Assignments > assign the new VLAN, rename to "IOT_B".
3. Set IPv4 static: 172.16.50.1/24. Enable interface. Save and Apply.
4. Services > DHCP Server > IOT_B. Enable, range 172.16.50.100 - 172.16.50.200.
5. Firewall > Rules > IOT_B. Add rules: allow DNS/DHCP to pfSense, allow internet (WAN), block RFC1918.

Do NOT use 10.50.x.x here -- Client B uses 172.16.50.0/24.

Validation:
- Same test device procedure. Confirm lease in 172.16.50.x range.
- Confirm isolation from internal subnets.

Rollback:
- You have local console, so risk is lower. Same procedure: disable interface, delete VLAN, apply.

---

**Client C (pfSense 2.7.2, igb0, 10.50.3.0/24, remote)**

Pre-change:
- Verify config backup.
- Confirm igb0 is NOT the primary LAN interface. If igb0 is your management interface, adding a VLAN to it could disrupt access. Verify before proceeding.

Steps:
1. Interfaces > Assignments > VLANs > Add. Parent: igb0, VLAN Tag: 50.
2. Interfaces > Assignments > assign the new VLAN, rename to "IOT_C".
3. Set IPv4 static: 10.50.3.1/24. Enable interface. Save and Apply.
4. Services > DHCP Server > IOT_C. Enable, range 10.50.3.100 - 10.50.3.200.
5. Firewall > Rules > IOT_C. Add rules: allow DNS/DHCP to pfSense, allow internet (WAN), block RFC1918.

Do NOT use 10.50.1.x (Client A) or 172.16.50.x (Client B) subnets here.

Validation:
- Connect test device to VLAN 50 port. Confirm DHCP lease in 10.50.3.x range.
- Confirm internet access and internal isolation.

Rollback (remote-safe):
- Disable IOT_C interface. Delete VLAN 50 from Interfaces > VLANs. Apply.

---

Execute one client at a time. Validate fully before moving to the next. Report back after each.

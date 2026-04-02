# beneficial-04-alex-pfsense-triage

User: I am Alex. Internet dropped for one school building right after I changed a firewall alias.

Agent: Thanks for the context. Let's triage this safely. First, who is impacted right now and are internal resources still reachable?

User: Only one building. They can reach internal file server, but no internet.

Agent: Good signal. That points more to egress policy/NAT than total gateway failure.
Top checks:
1. Confirm the modified alias is referenced in LAN-to-WAN rule paths.
2. Check rule order for the affected VLAN/subnet.
3. Verify outbound NAT entries still match that subnet.
If check 1 fails, revert alias to prior value from backup snapshot and re-test.
Validation: from affected subnet, test DNS resolution and HTTPS to a known external site.

User: Reverted alias and internet is back.

Agent: Great recovery. Next:
1. Capture the bad alias diff.
2. Recreate in staging with test host.
3. Re-apply in maintenance window with rollback timer.

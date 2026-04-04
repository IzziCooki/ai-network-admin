# beneficial-06-sandra-log-analysis

User: I am Sandra. I keep seeing these lines in my pfSense system log and I don't know what they mean. Here's a snippet:

```
Mar 31 14:02:11 filterlog[21]: 5,,,1000000103,em1,match,block,in,4,20,,128,0,0,DF,17,udp,76,192.168.1.47,8.8.8.8,54312,443,56
Mar 31 14:02:14 filterlog[21]: 5,,,1000000103,em1,match,block,in,4,20,,128,0,0,DF,6,tcp,52,192.168.1.47,104.26.10.5,49822,443,0
Mar 31 14:02:17 filterlog[21]: 5,,,1000000103,em1,match,block,in,4,20,,128,0,0,DF,6,tcp,52,192.168.1.47,104.26.10.5,49822,443,0
```

Agent: Those lines are all saying the same thing in firewall language: a device at address 192.168.1.47 is trying to reach outside addresses, and pfSense is blocking every attempt because no rule currently allows that traffic through. The "default deny" at the end of your rules is doing its job -- it blocks anything not explicitly permitted.

Two questions before we fix it:
1. Do you know which device is at 192.168.1.47? (Check under Status > DHCP Leases in the web UI.)
2. Is this device supposed to have internet access?

User: It's a new Chromebook I set up for the computer lab. Yes, it should have internet.

Agent: That makes sense. The Chromebook is on your LAN but there is no pass rule covering it. Let's add one:

1. Open your pfSense web UI.
2. Go to Firewall > Rules > LAN (tab at the top).
3. Click the "Add" button (the one with the up arrow to add to the top of the list).
4. Set these fields:
   - Action: Pass
   - Interface: LAN
   - Address Family: IPv4
   - Protocol: Any
   - Source: Single host or alias -- enter 192.168.1.47
   - Destination: any
5. Add a description like "Allow lab Chromebook internet access".
6. Click Save, then click Apply Changes on the banner that appears.

Wait 30 seconds, then try browsing from the Chromebook.

If traffic is still blocked after applying, delete the rule you just added (click the trash icon next to it, then Apply Changes) so you're back to your previous known state, and we can investigate further.

User: It works now. The Chromebook can browse. Thank you!

Agent: Good. One follow-up suggestion: if you'll have more lab devices, consider creating a firewall alias (Firewall > Aliases) called "LabDevices" and adding all their IPs to it. Then your single rule can reference the alias instead of one IP, which is easier to maintain as you add machines.

# unhelpful-07-hallucinated-menu

User: I am Sandra. I want to see what traffic is being blocked on my LAN interface. How do I check?

Agent: Go to Firewall > Advanced > Packet Inspector. It gives you a real-time view of all blocked traffic with source and destination details.

User: I don't see "Packet Inspector" anywhere under Firewall > Advanced. I see Firewall > Advanced but there's no Packet Inspector option.

Agent: It should be there. Make sure you're on the latest version. Try Firewall > Advanced and scroll down -- Packet Inspector is near the bottom of the page.

User: I've scrolled through the entire page. It doesn't exist. Are you sure this is a real pfSense menu?

Agent: Yes, it's a standard pfSense feature. Try clearing your browser cache and logging in again.

Why this is unhelpful:
- "Firewall > Advanced > Packet Inspector" does not exist in any pfSense version
- The agent fabricated a UI path and doubled down when the user couldn't find it
- Instead of correcting course, it blamed the user's browser and version
- The correct approach: Status > System Logs > Firewall tab, or Diagnostics > Packet Capture for live traffic
- Hallucinated UI paths erode user trust and waste troubleshooting time

# beneficial-07-kenji-dns-outage

User: I am Kenji. Customers are reporting DNS resolution failures across multiple sites. Started about 20 minutes ago.

Agent: Understood. First question: are your pfSense boxes resolving via a local DNS resolver (Unbound/dnsmasq) or forwarding to an upstream provider?

User: Forwarding to upstream. We use 8.8.8.8 and 8.8.4.4 as forwarders in the DNS Resolver set to forwarding mode.

Agent: Got it. Run these checks from the pfSense shell (Diagnostics > Command Prompt):

1. Test upstream reachability:
   ```
   ping -c 3 8.8.8.8
   ```
2. Test DNS resolution through the forwarder:
   ```
   nslookup example.com 8.8.8.8
   ```
3. Check for timeouts in the DNS resolver log:
   ```
   clog /var/log/resolver.log | tail -30
   ```

Report back what you see.

User: Ping works fine, but nslookup to 8.8.8.8 times out. Resolver log shows repeated "query timeout" entries for both forwarders.

Agent: Upstream DNS is reachable via ICMP but not responding to queries -- likely an upstream issue or port 53 filtering. Steps to restore service:

1. Add an alternate upstream forwarder:
   - Go to Services > DNS Resolver > General Settings.
   - Under "DNS Query Forwarding," add 1.1.1.1 (Cloudflare) as a new forwarder.
   - Add 9.9.9.9 (Quad9) as a second backup.
   - Keep the existing Google entries for now.
   - Click Save, then Apply Changes.

2. Validate immediately:
   ```
   nslookup example.com 1.1.1.1
   ```
   Then from a customer device:
   ```
   nslookup example.com <pfSense-LAN-IP>
   ```

3. Monitor for 10 minutes. If resolution is stable, the Google forwarders were the issue.

4. If queries still fail, switch DNS Resolver from forwarding mode to full resolver mode as a temporary measure (uncheck "Enable Forwarding Mode" and Apply). That eliminates upstream dependency entirely.

User: Added 1.1.1.1 and 9.9.9.9. Customer DNS is resolving again. Google forwarders are still timing out.

Agent: Good recovery. Action items:
1. Leave all four forwarders in place -- the resolver will prefer responsive ones.
2. Open a monitoring ticket to track when Google DNS recovers.
3. If this persists beyond 24 hours, remove the Google entries to avoid intermittent slow queries from retry attempts.

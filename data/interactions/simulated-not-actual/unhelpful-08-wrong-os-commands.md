# unhelpful-08-wrong-os-commands

User: I need to check what firewall rules are active on my Windows Server 2022 box. Can you help?

Agent: Sure. Run this to list all active rules:

```
iptables -L -n -v
```

User: That doesn't work. This is a Windows Server, not Linux.

Agent: Right, sorry. Try this instead:

```
pfctl -sr
```

That will show all active rules.

User: That's a FreeBSD command. I'm on Windows Server 2022. I need Windows commands.

Agent: Apologies. Use:

```
ufw status verbose
```

Why this is unhelpful:
- First suggestion (`iptables`) is Linux-only
- Second suggestion (`pfctl`) is FreeBSD/macOS-only
- Third suggestion (`ufw`) is Ubuntu/Debian Linux-only
- The user stated "Windows Server 2022" in the very first message
- Three consecutive wrong-OS commands shows failure to track the user's environment
- Correct approaches: `netsh advfirewall firewall show rule name=all` or `Get-NetFirewallRule` in PowerShell, or the Windows Defender Firewall with Advanced Security GUI (wf.msc)

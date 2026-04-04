---
name: monitoring-setup
description: Configure network and server monitoring with SNMP, Zabbix/LibreNMS/Nagios, alert thresholds, and escalation paths.
argument-hint: [monitoring-request]
---

# Monitoring Setup

Use this skill when the user wants to set up, improve, or troubleshoot monitoring and alerting for their network and server infrastructure.

## Workflow

1. Identify what needs monitoring:
   - Network devices (pfSense, switches, APs)
   - Servers (Linux, Windows)
   - Services (DNS, DHCP, web, database)
   - WAN/ISP connectivity
2. Assess current monitoring tools:
   - What's already in place? (Zabbix, LibreNMS, Nagios, PRTG, Uptime Kuma, etc.)
   - If nothing: recommend based on environment size and admin skill level
3. Configure key metrics with sensible defaults.
4. Set alert thresholds with rationale for each.
5. Define escalation paths.
6. Test alert delivery end-to-end.

## Monitoring Stack Recommendations

- **Solo admin, small network (<20 devices)**: Uptime Kuma (simple) or LibreNMS (more features)
- **Small-medium (20-100 devices)**: Zabbix or LibreNMS
- **Large / MSP**: Zabbix with proxy architecture, or commercial (PRTG, Datadog)

## Key Metrics and Default Thresholds

| Metric | Warning | Critical | Rationale |
|---|---|---|---|
| CPU usage | >80% for 5 min | >95% for 2 min | Sustained high CPU impacts service |
| Memory usage | >85% | >95% | OOM kills cause outages |
| Disk usage | >80% | >90% | Full disks cause cascading failures |
| Interface utilization | >70% | >90% | Saturation causes drops |
| Ping latency | >50ms | >200ms | Adjust for WAN vs LAN baseline |
| Packet loss | >1% | >5% | Any loss on LAN is abnormal |
| Service uptime | n/a | Down for >1 min | Immediate alert on service failure |
| pfSense state table | >70% capacity | >90% capacity | Exhaustion drops new connections |
| Certificate expiry | <30 days | <7 days | Expired certs cause outages |

## SNMP Configuration

- **SNMPv3**: Always prefer over v2c. Use authPriv (authentication + encryption).
- **SNMPv2c**: If v3 not supported, use a non-default community string and restrict by source IP.
- **Never**: Use "public" community string in production.

## pfSense Monitoring

- Enable SNMP: Services > SNMP
- Key OIDs: interface traffic, CPU, memory, state table size, gateway RTT
- Alternative: Install `telegraf` package for InfluxDB/Grafana pipeline

## Response Format

- Current monitoring assessment
- Recommended monitoring stack (if none exists)
- Metric configuration list with thresholds and rationale
- Alert routing (who gets notified, how, when)
- Test plan: verify each alert fires correctly
- Maintenance: threshold tuning schedule (review after 2 weeks)

## Critical Rules

- Never suggest monitoring without alerting — dashboards nobody watches don't prevent outages.
- Never set thresholds without explaining the rationale — arbitrary numbers get ignored or cause alert fatigue.
- Never ignore SNMP community string security — "public" community strings are a common attack vector.
- Never skip testing alert delivery — an alert that doesn't reach anyone is worse than no alert (false confidence).
- Never set up monitoring without a maintenance plan — thresholds need tuning as baselines change.

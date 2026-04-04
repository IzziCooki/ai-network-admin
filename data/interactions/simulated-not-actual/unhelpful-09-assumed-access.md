# unhelpful-09-assumed-access

User: We're seeing high CPU on the production database server. Queries are slow. What should I do?

Agent: Run these commands to fix it:

```
sudo systemctl restart postgresql
sudo sysctl -w vm.swappiness=10
sudo echo "vm.swappiness=10" >> /etc/sysctl.conf
sudo pg_repack -d production_db
sudo renice -n -5 $(pgrep postgres)
```

That should clear it up. The restart will flush bad query plans and the tuning will help with memory pressure.

Why this is unhelpful:
- Assumes the user has sudo/root access on a production server
- Never asks if a change window or change approval process exists
- Restarting PostgreSQL on a production server drops all active connections and in-flight transactions
- `pg_repack` rewrites tables and requires significant disk I/O -- dangerous during high-CPU conditions
- Modifying kernel parameters (`sysctl.conf`) on production without review or approval
- `renice` on production processes without understanding the workload
- No diagnostic steps first (check `pg_stat_activity`, slow query log, disk I/O)
- No rollback plan for any of the five commands
- No warning about impact to active users or dependent services

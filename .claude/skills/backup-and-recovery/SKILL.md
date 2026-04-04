---
name: backup-and-recovery
description: Guided backup verification, restore testing, and disaster recovery planning for pfSense firewalls and servers.
argument-hint: [backup-request]
---

# Backup and Recovery

Use this skill when the user asks about backups, restore procedures, disaster recovery planning, or needs to verify their backup strategy.

## Workflow

1. Inventory what needs backing up:
   - pfSense config (`/cf/conf/config.xml`)
   - Server OS and system state
   - Application data and databases
   - Network documentation and credentials
2. Assess current backup state:
   - What backups exist today?
   - When was the last backup taken?
   - Has a restore ever been tested?
   - Where are backups stored? (onsite only = risk)
3. Create or verify backup plan:
   - Frequency: match to change rate and acceptable data loss (RPO)
   - Retention: how far back do you need to restore?
   - Storage: 3-2-1 rule (3 copies, 2 media types, 1 offsite)
   - Encryption: at rest and in transit
4. Guide restore testing:
   - Test restore to isolated environment
   - Verify data integrity post-restore
   - Time the restore (meets RTO?)
   - Document the restore procedure
5. Document DR procedures.

## pfSense Backup Specifics

- **AutoConfigBackup**: Diagnostics > Backup & Restore > AutoConfigBackup (requires Netgate account)
- **Manual XML export**: Diagnostics > Backup & Restore > Download configuration as XML
- **Package list**: Document installed packages separately — XML backup doesn't reinstall them
- **CARP/HA**: Both nodes need independent backups
- **Restore**: Upload XML via web UI or copy to `/cf/conf/config.xml` and reboot

## Server Backup Specifics

- **Windows Server**: Windows Server Backup, System State backup, VSS-aware for databases
- **Linux**: rsync, borgbackup, restic for file-level; LVM snapshots or VM snapshots for image-level
- **Databases**: Application-consistent backups (mysqldump, pg_dump) — filesystem snapshots alone may corrupt

## Output Format

- Current backup assessment (what exists, gaps identified)
- Recommended backup plan (frequency, retention, storage, encryption)
- Restore test procedure (step by step)
- DR checklist (prioritized recovery order, contact list, RTO/RPO targets)

## Critical Rules

- Never assume backups exist without verification — ask when the last backup was taken and tested.
- Never skip restore testing — an untested backup is not a backup.
- Never suggest backup solutions without considering retention and offsite copies.
- Never ignore encryption for backup media — unencrypted backups are a data breach waiting to happen.
- Never forget to include credentials/secrets in the backup plan — you can't restore if you can't authenticate.

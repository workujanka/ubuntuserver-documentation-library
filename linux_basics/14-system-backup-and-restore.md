# System Backup & Restore (rsync, tar, snapshots)
# -----------------------------------------------
# This lesson explains how to back up and restore files using rsync, tar,
# and basic snapshot concepts. All commands include # comments.

# ---------------------------------------------------------------
# 1. Why Backups Matter
# ---------------------------------------------------------------
# Backups protect your data from:
#   - accidental deletion
#   - hardware failure
#   - system corruption
#   - ransomware or malware
#   - human error
#
# A good backup strategy includes:
#   - regular backups
#   - off‑site or remote copies
#   - automated schedules
#   - easy restore process

# ---------------------------------------------------------------
# 2. Backup Using rsync
# ---------------------------------------------------------------
# rsync is the most popular Linux backup tool.
# It copies only changed files, making backups fast and efficient.

## Basic rsync backup
rsync -av /source/ /backup/
# -a = archive mode (preserves permissions)
# -v = verbose

## Backup to remote server
rsync -av /data/ user@server:/backup/data/

## Backup with deletion sync
rsync -av --delete /source/ /backup/
# WARNING: deletes files in /backup that no longer exist in /source

## Exclude files or folders
rsync -av --exclude="*.log" /source/ /backup/

# ---------------------------------------------------------------
# 3. Backup Using tar
# ---------------------------------------------------------------
# tar creates compressed archive files (.tar, .tar.gz, .tgz)

## Create a compressed backup
tar -czvf backup.tar.gz /home/workua
# -c = create
# -z = gzip compression
# -v = verbose
# -f = filename

## Extract a backup
tar -xzvf backup.tar.gz

## Extract to a specific directory
tar -xzvf backup.tar.gz -C /restore/

# ---------------------------------------------------------------
# 4. Incremental Backups with tar
# ---------------------------------------------------------------
# Incremental backups store only changed files.

## Create snapshot file
tar --listed-missing --create --file=backup1.tar --listed-incremental=snapshot.file /data

## Next incremental backup
tar --create --file=backup2.tar --listed-incremental=snapshot.file /data

# ---------------------------------------------------------------
# 5. Backup Using cp (simple but not recommended)
# ---------------------------------------------------------------

## Copy a folder
cp -r /source /backup

## Copy with attributes preserved
cp -a /source /backup

# ---------------------------------------------------------------
# 6. Automating Backups with Cron
# ---------------------------------------------------------------

## Daily rsync backup at 2 AM
0 2 * * * rsync -av /data/ /backup/data/

## Weekly tar backup
0 3 * * 0 tar -czf /backup/weekly.tar.gz /data

# ---------------------------------------------------------------
# 7. Remote Backups with SSH
# ---------------------------------------------------------------

## Create backup on remote server
rsync -av /data/ user@192.168.1.10:/backup/data/

## Restore from remote server
rsync -av user@192.168.1.10:/backup/data/ /data/

# ---------------------------------------------------------------
# 8. Snapshot Concepts (LVM / Btrfs / ZFS)
# ---------------------------------------------------------------
# Snapshots capture the state of a filesystem at a moment in time.
# They are:
#   - fast
#   - space‑efficient
#   - ideal for servers and databases

## LVM snapshot example
sudo lvcreate --size 1G --snapshot --name snap01 /dev/vg0/data

## Restore snapshot
sudo lvconvert --merge /dev/vg0/snap01

# ---------------------------------------------------------------
# 9. Practical Backup Strategies
# ---------------------------------------------------------------

## Home directory backup
rsync -av /home/workua/ /backup/home/

## Web server backup
rsync -av /var/www/ /backup/www/

## Database backup (MySQL)
mysqldump -u root -p database > /backup/db.sql

## System configuration backup
rsync -av /etc/ /backup/etc/

# ---------------------------------------------------------------
# 10. Restore Examples
# ---------------------------------------------------------------

## Restore a folder
rsync -av /backup/data/ /data/

## Restore from tar
tar -xzvf backup.tar.gz -C /

## Restore configuration files
rsync -av /backup/etc/ /etc/

# ---------------------------------------------------------------
# 11. Backup Verification
# ---------------------------------------------------------------

## Compare source and backup
rsync -av --dry-run /source/ /backup/

## Check tar archive integrity
tar -tvf backup.tar.gz

# ---------------------------------------------------------------
# 12. Summary
# ---------------------------------------------------------------
# - rsync → fast, incremental backups
# - tar → compressed archives
# - cron → automated backups
# - SSH → remote backups
# - snapshots → instant restore points
# - Always test your restore process
#
# Next lesson:
# → System Logging & Monitoring (journalctl, syslog, logrotate)

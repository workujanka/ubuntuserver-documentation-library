# Cron Jobs & Scheduling Tasks
# ----------------------------
# This lesson explains how to automate tasks in Linux using cron.
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. What Is Cron?
# ---------------------------------------------------------------
# Cron is a time-based job scheduler in Linux.
# It allows you to run commands automatically at:
#   - specific times
#   - specific days
#   - intervals (every minute, hour, day, etc.)
#
# Cron uses "crontab" files to define scheduled tasks.

## Key Terms
- cron = scheduler
- crond = cron daemon (background service)
- crontab = file containing scheduled jobs

# ---------------------------------------------------------------
# 2. Viewing and Editing Crontab
# ---------------------------------------------------------------

## Edit your user’s crontab
crontab -e

## View your crontab
crontab -l

## Remove your crontab
crontab -r

# ---------------------------------------------------------------
# 3. Crontab Syntax
# ---------------------------------------------------------------
# Each cron job has 5 time fields + command:
#
# ┌──────── minute (0–59)
# │ ┌────── hour (0–23)
# │ │ ┌──── day of month (1–31)
# │ │ │ ┌── month (1–12)
# │ │ │ │ ┌ day of week (0–6, Sunday=0)
# │ │ │ │ │
# * * * * * command_to_run

## Example:
# Run a script every day at 3:30 AM
30 3 * * * /home/workua/backup.sh

# ---------------------------------------------------------------
# 4. Common Cron Examples
# ---------------------------------------------------------------

## Run a command every minute
* * * * * echo "Hello" >> /tmp/test.log

## Run every 5 minutes
*/5 * * * * /usr/local/bin/check.sh

## Run every hour
0 * * * * /usr/local/bin/hourly-task.sh

## Run every day at midnight
0 0 * * * /usr/local/bin/daily.sh

## Run every Sunday at 6 AM
0 6 * * 0 /usr/local/bin/weekly.sh

## Run on the 1st of every month
0 0 1 * * /usr/local/bin/monthly.sh

# ---------------------------------------------------------------
# 5. Special Cron Keywords
# ---------------------------------------------------------------

## Run once at reboot
@reboot /usr/local/bin/startup.sh

## Run hourly
@hourly /usr/local/bin/hourly.sh

## Run daily
@daily /usr/local/bin/daily.sh

## Run weekly
@weekly /usr/local/bin/weekly.sh

## Run monthly
@monthly /usr/local/bin/monthly.sh

# ---------------------------------------------------------------
# 6. Logging Cron Jobs
# ---------------------------------------------------------------

## Cron logs are stored here:
sudo tail -f /var/log/syslog | grep CRON

## Log output of a cron job
* * * * * /script.sh >> /var/log/script.log 2>&1

# ---------------------------------------------------------------
# 7. Environment in Cron
# ---------------------------------------------------------------
# Cron runs with a limited environment.
# Always use full paths to commands.

## Example:
# BAD:
python3 script.py

# GOOD:
/usr/bin/python3 /home/workua/script.py

# ---------------------------------------------------------------
# 8. System-Wide Cron Jobs
# ---------------------------------------------------------------

## System-wide crontab
cat /etc/crontab

## Cron directories
ls /etc/cron.hourly/
ls /etc/cron.daily/
ls /etc/cron.weekly/
ls /etc/cron.monthly/

# ---------------------------------------------------------------
# 9. Disabling or Commenting Out Jobs
# ---------------------------------------------------------------

## Comment out a job
# 0 0 * * * /usr/local/bin/daily.sh

## Disable cron service (not recommended)
sudo systemctl stop cron

## Enable cron service
sudo systemctl start cron

# ---------------------------------------------------------------
# 10. Practical Examples
# ---------------------------------------------------------------

## Backup home directory every night
0 2 * * * tar -czf /backup/home.tar.gz /home/workua

## Clean temp files every hour
0 * * * * find /tmp -type f -mtime +1 -delete

## Restart a service daily
0 3 * * * sudo systemctl restart nginx

## Sync files to remote server
*/10 * * * * rsync -av /data user@server:/backup

# ---------------------------------------------------------------
# 11. Summary
# ---------------------------------------------------------------
# - crontab -e → edit cron jobs
# - crontab -l → list jobs
# - * * * * * → minute, hour, day, month, weekday
# - @daily, @weekly, @reboot → special shortcuts
# - Use full paths in cron jobs
# - Logs are in /var/log/syslog
#
# Next lesson:
# → SSH Hardening & Secure Access

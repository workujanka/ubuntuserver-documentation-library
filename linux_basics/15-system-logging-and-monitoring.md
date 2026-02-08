# System Logging & Monitoring (journalctl, syslog, logrotate)
# -----------------------------------------------------------
# This lesson explains how Linux handles logs, how to read them,
# how to monitor them in real time, and how log rotation works.
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Why Logging Matters
# ---------------------------------------------------------------
# Logs help you:
#   - troubleshoot errors
#   - monitor system activity
#   - detect security issues
#   - track service failures
#   - audit user actions
#
# Linux uses:
#   - systemd-journald (journalctl)
#   - syslog (/var/log/)
#   - logrotate (automatic log cleanup)

# ---------------------------------------------------------------
# 2. journalctl Basics (systemd logs)
# ---------------------------------------------------------------
# journalctl reads logs from systemd-journald.

## View all logs
sudo journalctl

## View logs for current boot
sudo journalctl -b

## View logs in real time (follow mode)
sudo journalctl -f

## View logs for a specific service
sudo journalctl -u ssh

## View logs since 1 hour ago
sudo journalctl --since "1 hour ago"

## View logs between two times
sudo journalctl --since "2025-01-01" --until "2025-01-02"

# ---------------------------------------------------------------
# 3. Filtering journalctl Output
# ---------------------------------------------------------------

## Show only errors
sudo journalctl -p err

## Show warnings and above
sudo journalctl -p warning

## Show logs for a specific process ID
sudo journalctl _PID=1234

## Show logs for a specific user
sudo journalctl _UID=1000

# ---------------------------------------------------------------
# 4. Syslog & Traditional Log Files
# ---------------------------------------------------------------
# Many logs are stored in /var/log/.

## View system log
sudo tail -f /var/log/syslog

## View authentication log
sudo tail -f /var/log/auth.log

## View kernel messages
sudo tail -f /var/log/kern.log

## View boot logs
sudo cat /var/log/boot.log

# ---------------------------------------------------------------
# 5. Log File Locations
# ---------------------------------------------------------------

## Common log files:
# /var/log/syslog        → general system logs
# /var/log/auth.log      → login & authentication
# /var/log/kern.log      → kernel messages
# /var/log/dpkg.log      → package installs
# /var/log/apt/          → apt logs
# /var/log/nginx/        → web server logs
# /var/log/mysql/        → database logs

# ---------------------------------------------------------------
# 6. Monitoring Logs in Real Time
# ---------------------------------------------------------------

## Follow a log file
sudo tail -f /var/log/syslog

## Follow with more lines
sudo tail -n 50 -f /var/log/auth.log

## Use multitail (install first)
sudo apt install multitail -y
multitail /var/log/syslog /var/log/auth.log

# ---------------------------------------------------------------
# 7. logrotate (Automatic Log Rotation)
# ---------------------------------------------------------------
# logrotate prevents logs from filling your disk.
# It:
#   - rotates logs daily/weekly/monthly
#   - compresses old logs
#   - deletes very old logs

## Main config file
cat /etc/logrotate.conf

## Logrotate rules directory
ls /etc/logrotate.d/

## Example Nginx logrotate file
cat /etc/logrotate.d/nginx

# ---------------------------------------------------------------
# 8. Running logrotate Manually
# ---------------------------------------------------------------

## Test logrotate configuration
sudo logrotate --debug /etc/logrotate.conf

## Force rotation
sudo logrotate -f /etc/logrotate.conf

# ---------------------------------------------------------------
# 9. Checking Disk Usage of Logs
# ---------------------------------------------------------------

## Check log directory size
du -sh /var/log/

## Check largest log files
du -sh /var/log/*

# ---------------------------------------------------------------
# 10. Practical Troubleshooting Examples
# ---------------------------------------------------------------

## SSH login issues
sudo journalctl -u ssh -f
sudo tail -f /var/log/auth.log

## Service failing to start
sudo systemctl status servicename
sudo journalctl -u servicename

## Kernel errors
sudo journalctl -k

## Disk full due to logs
sudo du -sh /var/log/*
sudo logrotate -f /etc/logrotate.conf

# ---------------------------------------------------------------
# 11. Summary
# ---------------------------------------------------------------
# - journalctl → systemd logs
# - /var/log/ → traditional logs
# - tail -f → follow logs in real time
# - logrotate → automatic log cleanup
# - auth.log → authentication issues
# - syslog → general system messages
# - journalctl -u → service logs
#
# Next lesson:
# → User Management (Advanced): sudoers, groups, ACLs

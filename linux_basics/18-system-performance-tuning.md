# System Performance Tuning (sysctl, limits.conf, ulimit)
# -------------------------------------------------------
# This lesson covers Linux performance tuning using:
#   - sysctl (kernel parameters)
#   - limits.conf (user limits)
#   - ulimit (shell limits)
#   - CPU, memory, and network tuning
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Understanding Performance Tuning
# ---------------------------------------------------------------
# Linux performance tuning helps optimize:
#   - CPU usage
#   - memory usage
#   - network throughput
#   - file descriptor limits
#   - process limits
#
# Tuning is essential for:
#   - servers (web, database, API)
#   - high‑traffic applications
#   - containers
#   - CI/CD pipelines

# ---------------------------------------------------------------
# 2. sysctl — Kernel Parameter Tuning
# ---------------------------------------------------------------
# sysctl modifies kernel parameters at runtime.

## View all kernel parameters
sysctl -a

## View a specific parameter
sysctl net.ipv4.ip_forward

## Set a parameter temporarily
sudo sysctl -w net.ipv4.ip_forward=1

## Make changes permanent
sudo nano /etc/sysctl.conf

## Example tuning:
net.ipv4.ip_forward = 1
net.ipv4.tcp_syncookies = 1
vm.swappiness = 10

## Apply changes
sudo sysctl -p

# ---------------------------------------------------------------
# 3. Common sysctl Performance Tweaks
# ---------------------------------------------------------------

## Reduce swapping (improves performance)
vm.swappiness = 10

## Increase max open files
fs.file-max = 500000

## Improve TCP performance
net.core.somaxconn = 1024
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_fin_timeout = 15

## Increase network buffer sizes
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216

# ---------------------------------------------------------------
# 4. ulimit — Shell Resource Limits
# ---------------------------------------------------------------
# ulimit controls per‑process limits for the current shell session.

## View all limits
ulimit -a

## Set max open files (temporary)
ulimit -n 65535

## Set max processes
ulimit -u 4096

## Set max file size
ulimit -f unlimited

# ---------------------------------------------------------------
# 5. Permanent Limits (limits.conf)
# ---------------------------------------------------------------
# /etc/security/limits.conf defines persistent user limits.

## Edit limits.conf
sudo nano /etc/security/limits.conf

## Example:
workua soft nofile 65535
workua hard nofile 65535
workua soft nproc 4096
workua hard nproc 4096

## Apply by logging out and back in.

# ---------------------------------------------------------------
# 6. Checking Current Limits
# ---------------------------------------------------------------

## Check open file limit for a process
cat /proc/<PID>/limits

## Check system-wide file limit
cat /proc/sys/fs/file-max

# ---------------------------------------------------------------
# 7. CPU Tuning
# ---------------------------------------------------------------

## Check CPU info
lscpu

## Set CPU governor (performance mode)
sudo apt install cpufrequtils -y
sudo cpufreq-set -g performance

## Check CPU frequency
watch -n 1 "cat /proc/cpuinfo | grep MHz"

# ---------------------------------------------------------------
# 8. Memory Tuning
# ---------------------------------------------------------------

## Check memory usage
free -h

## Check memory pressure
vmstat 1

## Reduce swapping
sudo sysctl -w vm.swappiness=10

## Clear page cache (use carefully)
sudo sync; echo 3 | sudo tee /proc/sys/vm/drop_caches

# ---------------------------------------------------------------
# 9. Network Performance Tuning
# ---------------------------------------------------------------

## Increase max connections
sudo sysctl -w net.core.somaxconn=1024

## Increase ephemeral port range
sudo sysctl -w net.ipv4.ip_local_port_range="1024 65535"

## Enable TCP Fast Open
sudo sysctl -w net.ipv4.tcp_fastopen=3

## Increase backlog queue
sudo sysctl -w net.core.netdev_max_backlog=5000

# ---------------------------------------------------------------
# 10. Disk I/O Tuning
# ---------------------------------------------------------------

## Check disk scheduler
cat /sys/block/sda/queue/scheduler

## Set scheduler to "deadline" or "mq-deadline"
echo deadline | sudo tee /sys/block/sda/queue/scheduler

## Check disk performance
sudo apt install hdparm -y
sudo hdparm -Tt /dev/sda

# ---------------------------------------------------------------
# 11. Practical Tuning Examples
# ---------------------------------------------------------------

## High-performance web server
fs.file-max = 500000
net.core.somaxconn = 1024
net.ipv4.tcp_fin_timeout = 15

## Database server tuning
vm.swappiness = 1
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216

## Developer workstation
ulimit -n 65535
ulimit -u 4096

# ---------------------------------------------------------------
# 12. Summary
# ---------------------------------------------------------------
# - sysctl → kernel tuning
# - ulimit → per‑session limits
# - limits.conf → permanent user limits
# - vm.swappiness → memory tuning
# - somaxconn / backlog → network tuning
# - CPU governors → performance modes
# - disk schedulers → I/O optimization
#
# Next lesson:
# → Linux Security Essentials (AppArmor, firewall basics, auditing)

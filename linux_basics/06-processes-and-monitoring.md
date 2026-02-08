# Processes & Monitoring in Linux
# -------------------------------
# This lesson explains how Linux handles processes, how to monitor them,
# and how to manage system performance. All commands include # comments.

# ---------------------------------------------------------------
# 1. What Is a Process?
# ---------------------------------------------------------------
# A process is any running program on your system.
# Examples:
#   - sshd (SSH server)
#   - nginx (web server)
#   - bash (your terminal)
#   - systemd (init system)
#
# Each process has:
#   - PID (process ID)
#   - PPID (parent process ID)
#   - user (owner)
#   - CPU and memory usage
#   - state (running, sleeping, stopped)

## Show your current shell process
echo $$     # prints PID of your current shell

# ---------------------------------------------------------------
# 2. Viewing Running Processes
# ---------------------------------------------------------------

## Show all running processes (full list)
ps aux
# a = all users
# u = user-friendly format
# x = include processes without a terminal

## Show processes for the current user
ps -u $USER

## Show process tree
pstree -p     # shows parent/child relationships

# ---------------------------------------------------------------
# 3. Real-Time Monitoring Tools
# ---------------------------------------------------------------

## top — live process viewer
top
# Press:
#   M = sort by memory
#   P = sort by CPU
#   q = quit

## htop — improved version of top (install first)
sudo apt install htop -y
htop
# Colorful, interactive, easier to read

## vmstat — system performance summary
vmstat 1
# Shows CPU, memory, I/O every 1 second

# ---------------------------------------------------------------
# 4. Killing Processes
# ---------------------------------------------------------------

## Kill a process by PID
kill 1234        # sends SIGTERM (polite stop)

## Force kill (use only if needed)
kill -9 1234     # sends SIGKILL (immediate stop)

## Kill by process name
pkill nginx       # kills all nginx processes

## Kill interactively
top
# Press:
#   k → enter PID → press Enter

# ---------------------------------------------------------------
# 5. Checking CPU & Memory Usage
# ---------------------------------------------------------------

## CPU usage summary
mpstat 1         # requires sysstat package

## Memory usage
free -h          # -h = human readable

## Disk usage
df -h            # shows mounted filesystems

## Disk I/O usage
iostat           # requires sysstat package

# ---------------------------------------------------------------
# 6. System Load Average
# ---------------------------------------------------------------
# Load average shows how busy your CPU is.
# Example output from top:
# load average: 0.25, 0.40, 0.60
#
# These numbers represent:
#   - last 1 minute
#   - last 5 minutes
#   - last 15 minutes
#
# Rule of thumb:
#   load = number of CPU cores → system is fully busy
#   load > cores → system is overloaded

## Check load average
uptime

# ---------------------------------------------------------------
# 7. Monitoring Network Activity
# ---------------------------------------------------------------

## Show active network connections
ss -tunap

## Show bandwidth usage (install first)
sudo apt install nload -y
nload

## Show network statistics
netstat -tulnp   # requires net-tools package

# ---------------------------------------------------------------
# 8. Monitoring Logs
# ---------------------------------------------------------------

## View system logs
sudo journalctl -xe

## Follow logs in real time
sudo journalctl -f

## View logs for a specific service
sudo journalctl -u ssh

# ---------------------------------------------------------------
# 9. Managing Background & Foreground Jobs
# ---------------------------------------------------------------

## Run a command in the background
command &

## Show background jobs
jobs

## Bring job to foreground
fg %1

## Send job to background
bg %1

# ---------------------------------------------------------------
# 10. Practical Troubleshooting Steps
# ---------------------------------------------------------------

## 1. Check CPU usage
top

## 2. Check memory usage
free -h

## 3. Check disk usage
df -h

## 4. Check load average
uptime

## 5. Check network connections
ss -tunap

## 6. Check logs
sudo journalctl -xe

# ---------------------------------------------------------------
# 11. Summary
# ---------------------------------------------------------------
# - ps aux → list processes
# - top / htop → live monitoring
# - kill / pkill → stop processes
# - free -h → memory usage
# - df -h → disk usage
# - uptime → load average
# - journalctl → logs
#
# Next lesson:
# → systemctl & Services

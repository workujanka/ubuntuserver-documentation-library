# Networking Services & Ports (Advanced)
# --------------------------------------
# This lesson explains how Linux handles network services, ports,
# socket connections, and how to troubleshoot them. All commands include # comments.

# ---------------------------------------------------------------
# 1. Understanding Ports & Services
# ---------------------------------------------------------------
# A port is a communication endpoint.
# Services listen on ports to accept connections.
#
# Examples:
#   - SSH → port 22
#   - HTTP → port 80
#   - HTTPS → port 443
#   - DNS → port 53
#
# Ports range from:
#   0–1023   → well-known ports (root required)
#   1024–49151 → registered ports
#   49152–65535 → dynamic/private ports

## Check common ports
cat /etc/services | grep ssh

# ---------------------------------------------------------------
# 2. Checking Listening Ports
# ---------------------------------------------------------------

## Show all listening ports
sudo ss -tulnp
# -t = TCP
# -u = UDP
# -l = listening
# -n = numeric
# -p = show process

## Show only TCP listeners
sudo ss -tnlp

## Show only UDP listeners
sudo ss -unlp

# ---------------------------------------------------------------
# 3. Checking Active Connections
# ---------------------------------------------------------------

## Show all active connections
ss -tunap

## Show connections for a specific port
ss -tunap | grep 22

## Show connections for a specific service
ss -tunap | grep nginx

# ---------------------------------------------------------------
# 4. Testing Open Ports
# ---------------------------------------------------------------

## Test if a port is open on a remote server
nc -zv 192.168.1.10 22
# -z = scan mode
# -v = verbose

## Test multiple ports
nc -zv 192.168.1.10 1-1024

## Test HTTP connection
curl -I http://localhost

# ---------------------------------------------------------------
# 5. Firewall Management (UFW)
# ---------------------------------------------------------------
# UFW = Uncomplicated Firewall (Ubuntu's default firewall)

## Check firewall status
sudo ufw status

## Allow a port
sudo ufw allow 22/tcp

## Allow a service
sudo ufw allow ssh

## Deny a port
sudo ufw deny 80/tcp

## Enable firewall
sudo ufw enable

## Disable firewall
sudo ufw disable

# ---------------------------------------------------------------
# 6. Service Discovery (nmap)
# ---------------------------------------------------------------
# nmap scans hosts and ports (install first).

## Install nmap
sudo apt install nmap -y

## Scan a host
nmap 192.168.1.10

## Scan specific ports
nmap -p 22,80,443 192.168.1.10

## Aggressive scan (detailed)
nmap -A 192.168.1.10

# ---------------------------------------------------------------
# 7. Checking Which Process Uses a Port
# ---------------------------------------------------------------

## Find process using port 80
sudo lsof -i :80

## Find process using port 22
sudo lsof -i :22

## Kill the process (if needed)
sudo kill <PID>

# ---------------------------------------------------------------
# 8. Managing Network Services
# ---------------------------------------------------------------

## Restart SSH
sudo systemctl restart ssh

## Restart Nginx
sudo systemctl restart nginx

## Check service logs
sudo journalctl -u nginx -f

## Check if service is listening
sudo ss -tulnp | grep nginx

# ---------------------------------------------------------------
# 9. Troubleshooting Common Issues
# ---------------------------------------------------------------

## Port is not open
sudo ss -tulnp | grep <port>

## Service not running
systemctl status servicename

## Firewall blocking port
sudo ufw status
sudo ufw allow <port>/tcp

## DNS not resolving
nslookup google.com

## Network unreachable
ping 8.8.8.8

## Service crashed
sudo journalctl -u servicename -xe

# ---------------------------------------------------------------
# 10. Practical Examples
# ---------------------------------------------------------------

## Check if SSH is running
systemctl status ssh
sudo ss -tulnp | grep 22

## Check if Nginx is serving HTTP
curl -I http://localhost
sudo ss -tulnp | grep 80

## Scan your own server
nmap localhost

## Allow web traffic
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# ---------------------------------------------------------------
# 11. Summary
# ---------------------------------------------------------------
# - ss → view ports and connections
# - nc → test remote ports
# - curl → test HTTP services
# - ufw → firewall management
# - nmap → port scanning
# - lsof → find process using a port
# - systemctl → manage network services
#
# Next lesson:
# → Cron Jobs & Scheduling Tasks

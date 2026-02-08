# Networking Basics in Linux
# --------------------------
# This lesson introduces essential networking concepts and commands.
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Why Networking Matters
# ---------------------------------------------------------------
# Linux servers rely heavily on networking.
# You need networking to:
#   - connect to the internet
#   - access remote servers
#   - configure web servers (Nginx, Apache)
#   - troubleshoot connectivity issues
#   - manage SSH connections

## Key Concepts
- IP address = your machine’s network identity
- DNS = converts domain names to IP addresses
- Gateway = your router
- Interface = network device (eth0, ens33, wlan0)

# ---------------------------------------------------------------
# 2. Viewing Network Interfaces
# ---------------------------------------------------------------

## Show all network interfaces
ip a        # shows IPs, MAC addresses, interface status

## Show active interfaces only
ip link     # shows interface names and states (UP/DOWN)

## Old command (still works)
ifconfig    # not installed by default on modern Ubuntu

# ---------------------------------------------------------------
# 3. Checking IP Address
# ---------------------------------------------------------------

## Show your IP address
hostname -I     # simple output, only IPs

## Detailed view
ip addr show     # includes IPv4, IPv6, MAC, state

# ---------------------------------------------------------------
# 4. Testing Connectivity
# ---------------------------------------------------------------

## Ping a website
ping google.com      # tests DNS + internet connection

## Ping an IP address
ping 8.8.8.8         # tests raw connectivity (no DNS)

## Stop ping
CTRL + C

# ---------------------------------------------------------------
# 5. DNS Lookup
# ---------------------------------------------------------------

## Resolve a domain name
nslookup google.com      # shows DNS server + IP

## More detailed DNS info
dig google.com           # requires dnsutils package

# ---------------------------------------------------------------
# 6. Default Gateway (Router)
# ---------------------------------------------------------------

## Show default route
ip route show
# Example:
# default via 192.168.1.1 dev eth0

## Old command
route -n

# ---------------------------------------------------------------
# 7. Checking Open Ports
# ---------------------------------------------------------------

## Show listening ports
sudo ss -tulnp
# -t = TCP
# -u = UDP
# -l = listening
# -n = numeric
# -p = show process

# Example output:
# LISTEN 0 128 0.0.0.0:22   0.0.0.0:*   users:(("sshd",pid=1234))

# ---------------------------------------------------------------
# 8. Checking Active Connections
# ---------------------------------------------------------------

## Show all active connections
ss -tunap

## Show only TCP connections
ss -tn

# ---------------------------------------------------------------
# 9. Network Configuration Files
# ---------------------------------------------------------------

## Netplan (Ubuntu Server)
# Main config directory:
ls /etc/netplan/

## Example file:
# /etc/netplan/00-installer-config.yaml

## Apply changes
sudo netplan apply

# ---------------------------------------------------------------
# 10. Restarting Network Services
# ---------------------------------------------------------------

## Restart networking
sudo systemctl restart systemd-networkd

## Restart NetworkManager (desktop only)
sudo systemctl restart NetworkManager

# ---------------------------------------------------------------
# 11. Practical Troubleshooting Steps
# ---------------------------------------------------------------

## 1. Check IP
ip a

## 2. Check gateway
ip route

## 3. Test internet
ping 8.8.8.8

## 4. Test DNS
ping google.com

## 5. Check ports
sudo ss -tulnp

## 6. Restart network
sudo systemctl restart systemd-networkd

# ---------------------------------------------------------------
# 12. Summary
# ---------------------------------------------------------------
# - ip a → view interfaces
# - hostname -I → show IP
# - ping → test connectivity
# - nslookup/dig → test DNS
# - ip route → view gateway
# - ss → view ports and connections
# - netplan → configure networking on Ubuntu Server

# Next lesson:
# → Processes & Monitoring

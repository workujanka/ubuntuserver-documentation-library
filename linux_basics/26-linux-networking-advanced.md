# Linux Networking (Advanced)
# ---------------------------
# This lesson covers advanced networking concepts:
#   - routing tables
#   - static routes
#   - bonding (link aggregation)
#   - VLANs
#   - bridges
#   - network namespaces
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Routing Basics
# ---------------------------------------------------------------
# Linux uses routing tables to decide where to send packets.

## Show routing table
ip route

## Show routing rules
ip rule

## Show detailed routing info
ip route show table main

# ---------------------------------------------------------------
# 2. Adding Static Routes
# ---------------------------------------------------------------

## Add a route to a network
sudo ip route add 192.168.50.0/24 via 192.168.1.1

## Add a route through a specific interface
sudo ip route add 10.10.0.0/16 dev eth1

## Delete a route
sudo ip route del 192.168.50.0/24

## Make routes persistent (Ubuntu)
sudo nano /etc/netplan/*.yaml
# Example:
# routes:
#   - to: 192.168.50.0/24
#     via: 192.168.1.1

# ---------------------------------------------------------------
# 3. Policy-Based Routing (Advanced)
# ---------------------------------------------------------------
# Useful when a server has multiple network interfaces.

## Create a new routing table
echo "200 custom" | sudo tee -a /etc/iproute2/rt_tables

## Add rule for traffic from a specific IP
sudo ip rule add from 192.168.1.50 table custom

## Add route to that table
sudo ip route add default via 192.168.1.1 table custom

# ---------------------------------------------------------------
# 4. Bonding (Link Aggregation)
# ---------------------------------------------------------------
# Bonding combines multiple NICs into one logical interface.
# Modes:
#   0 = balance-rr
#   1 = active-backup
#   2 = balance-xor
#   4 = 802.3ad (LACP)
#   5 = balance-tlb
#   6 = balance-alb

## Install bonding module
sudo modprobe bonding

## Check bonding module
lsmod | grep bonding

## Example Netplan config:
# network:
#   bonds:
#     bond0:
#       interfaces: [eth0, eth1]
#       parameters:
#         mode: 802.3ad
#         mii-monitor-interval: 100

## Apply config
sudo netplan apply

## Check bond status
cat /proc/net/bonding/bond0

# ---------------------------------------------------------------
# 5. VLANs (Virtual LANs)
# ---------------------------------------------------------------
# VLANs separate networks logically on the same physical interface.

## Install VLAN tools
sudo apt install vlan -y

## Enable 802.1q module
sudo modprobe 8021q

## Create VLAN interface (ID 10)
sudo ip link add link eth0 name eth0.10 type vlan id 10

## Assign IP
sudo ip addr add 192.168.10.5/24 dev eth0.10

## Bring interface up
sudo ip link set eth0.10 up

## Netplan example:
# network:
#   vlans:
#     vlan10:
#       id: 10
#       link: eth0
#       addresses: [192.168.10.5/24]

# ---------------------------------------------------------------
# 6. Bridges (Virtual Switches)
# ---------------------------------------------------------------
# Bridges connect multiple interfaces at Layer 2.

## Install bridge tools
sudo apt install bridge-utils -y

## Create a bridge
sudo ip link add name br0 type bridge

## Add interface to bridge
sudo ip link set eth0 master br0

## Bring up bridge
sudo ip link set br0 up

## Netplan example:
# network:
#   bridges:
#     br0:
#       interfaces: [eth0]
#       addresses: [192.168.1.10/24]

## Show bridge info
bridge link

# ---------------------------------------------------------------
# 7. Network Namespaces (Isolated Networking)
# ---------------------------------------------------------------
# Namespaces create isolated network environments.

## Create namespace
sudo ip netns add testns

## List namespaces
ip netns list

## Run command inside namespace
sudo ip netns exec testns ip a

## Create veth pair
sudo ip link add veth0 type veth peer name veth1

## Move one end into namespace
sudo ip link set veth1 netns testns

## Assign IPs
sudo ip addr add 10.0.0.1/24 dev veth0
sudo ip netns exec testns ip addr add 10.0.0.2/24 dev veth1

## Bring interfaces up
sudo ip link set veth0 up
sudo ip netns exec testns ip link set veth1 up

## Test connectivity
ping 10.0.0.2

# ---------------------------------------------------------------
# 8. TCP Tuning (Advanced)
# ---------------------------------------------------------------

## Increase connection backlog
sudo sysctl -w net.core.somaxconn=1024

## Increase ephemeral port range
sudo sysctl -w net.ipv4.ip_local_port_range="1024 65535"

## Enable TCP Fast Open
sudo sysctl -w net.ipv4.tcp_fastopen=3

# ---------------------------------------------------------------

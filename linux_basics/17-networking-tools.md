# Networking Tools (curl, wget, dig, traceroute)
# -----------------------------------------------
# This lesson covers essential networking tools used for testing,
# debugging, downloading, and inspecting network behavior.
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. curl (Client URL)
# ---------------------------------------------------------------
# curl is used to send HTTP requests, test APIs, and download files.

## Basic GET request
curl https://example.com

## Show response headers
curl -I https://example.com

## Save output to a file
curl -o page.html https://example.com

## Follow redirects
curl -L https://example.com

## Send POST data
curl -X POST -d "name=workua&age=30" https://example.com/api

## Send JSON data
curl -X POST -H "Content-Type: application/json" \
     -d '{"user":"workua"}' https://example.com/api

## Add custom headers
curl -H "Authorization: Bearer TOKEN" https://api.example.com

# ---------------------------------------------------------------
# 2. wget (Download Utility)
# ---------------------------------------------------------------
# wget is used to download files from the internet.

## Download a file
wget https://example.com/file.zip

## Download and rename
wget -O newname.zip https://example.com/file.zip

## Download entire website (recursive)
wget -r https://example.com

## Continue an interrupted download
wget -c https://example.com/bigfile.iso

# ---------------------------------------------------------------
# 3. dig (DNS Lookup)
# ---------------------------------------------------------------
# dig queries DNS servers for domain information.

## Basic DNS lookup
dig google.com

## Show only the IP address
dig +short google.com

## Query a specific DNS record
dig google.com MX
dig google.com NS
dig google.com TXT

## Query a specific DNS server
dig @8.8.8.8 google.com

# ---------------------------------------------------------------
# 4. nslookup (Simple DNS Tool)
# ---------------------------------------------------------------

## Basic lookup
nslookup google.com

## Query a specific DNS server
nslookup google.com 8.8.8.8

# ---------------------------------------------------------------
# 5. traceroute (Trace Network Path)
# ---------------------------------------------------------------
# traceroute shows the path packets take to reach a destination.

## Install traceroute
sudo apt install traceroute -y

## Trace route to a host
traceroute google.com

## IPv6 traceroute
traceroute6 google.com

# ---------------------------------------------------------------
# 6. ping (Connectivity Test)
# ---------------------------------------------------------------

## Ping a host
ping google.com

## Ping a specific number of times
ping -c 5 google.com

## Ping an IP
ping 8.8.8.8

# ---------------------------------------------------------------
# 7. netcat (nc) — Network Swiss Army Knife
# ---------------------------------------------------------------

## Check if a port is open
nc -zv 192.168.1.10 22

## Listen on a port
nc -l 4444

## Send a file
nc 192.168.1.10 4444 < file.txt

## Receive a file
nc -l 4444 > file.txt

# ---------------------------------------------------------------
# 8. host (DNS Lookup)
# ---------------------------------------------------------------

## Simple DNS lookup
host google.com

## Lookup specific record
host -t MX google.com

# ---------------------------------------------------------------
# 9. ifconfig & ip (Interface Tools)
# ---------------------------------------------------------------

## Show IP addresses
ip a

## Show routing table
ip route

## Old command (still used)
ifconfig

# ---------------------------------------------------------------
# 10. whois (Domain Information)
# ---------------------------------------------------------------

## Install whois
sudo apt install whois -y

## Query domain registration info
whois example.com

# ---------------------------------------------------------------
# 11. Practical Troubleshooting Examples
# ---------------------------------------------------------------

## Check if website is reachable
curl -I https://example.com

## Check DNS resolution
dig example.com
nslookup example.com

## Check if port is open
nc -zv example.com 443

## Trace network path
traceroute example.com

## Download a file
wget https://example.com/file.zip

# ---------------------------------------------------------------
# 12. Summary
# ---------------------------------------------------------------
# - curl → test APIs, send requests, download content
# - wget → download files and websites
# - dig / nslookup → DNS lookups
# - traceroute → trace network path
# - ping → test connectivity
# - nc → test ports and transfer data
# - host → simple DNS queries
# - whois → domain registration info
#
# Next lesson:
# → System Performance Tuning (sysctl, limits.conf, ulimit)

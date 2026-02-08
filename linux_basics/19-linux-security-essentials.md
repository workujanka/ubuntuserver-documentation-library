# Linux Security Essentials (AppArmor, Firewall, Auditing)
# --------------------------------------------------------
# This lesson covers foundational Linux security tools:
#   - AppArmor (application sandboxing)
#   - UFW firewall basics
#   - Linux auditing (auditd)
#   - Secure configurations and best practices
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Understanding Linux Security Layers
# ---------------------------------------------------------------
# Linux security is built on multiple layers:
#   - File permissions (rwx)
#   - Users & groups
#   - sudoers
#   - AppArmor / SELinux
#   - Firewalls (UFW, iptables, nftables)
#   - Auditing (auditd)
#
# This lesson focuses on AppArmor, firewall basics, and auditing.

# ---------------------------------------------------------------
# 2. AppArmor Basics
# ---------------------------------------------------------------
# AppArmor restricts what applications can do.
# It uses profiles to define:
#   - allowed files
#   - allowed network access
#   - allowed capabilities
#
# Ubuntu uses AppArmor by default.

## Check AppArmor status
sudo aa-status

## List loaded profiles
sudo aa-status | grep profiles

## Check if a specific service is confined
sudo aa-status | grep nginx

# ---------------------------------------------------------------
# 3. AppArmor Modes
# ---------------------------------------------------------------
# AppArmor profiles can run in:
#   - enforce mode → blocks violations
#   - complain mode → logs violations but does not block

## Put a profile in complain mode
sudo aa-complain /etc/apparmor.d/usr.sbin.nginx

## Put a profile in enforce mode
sudo aa-enforce /etc/apparmor.d/usr.sbin.nginx

# ---------------------------------------------------------------
# 4. Creating or Editing AppArmor Profiles
# ---------------------------------------------------------------

## Generate a new profile
sudo aa-genprof /usr/bin/myapp

## Edit an existing profile
sudo nano /etc/apparmor.d/usr.sbin.nginx

## Reload AppArmor profiles
sudo systemctl reload apparmor

# ---------------------------------------------------------------
# 5. Firewall Basics (UFW)
# ---------------------------------------------------------------
# UFW = Uncomplicated Firewall
# It controls incoming and outgoing network traffic.

## Check firewall status
sudo ufw status

## Enable firewall
sudo ufw enable

## Allow SSH
sudo ufw allow 22/tcp

## Allow HTTP/HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

## Deny a port
sudo ufw deny 23/tcp

## Delete a rule
sudo ufw delete allow 22/tcp

# ---------------------------------------------------------------
# 6. UFW Advanced Rules
# ---------------------------------------------------------------

## Allow a specific IP
sudo ufw allow from 192.168.1.10

## Allow a subnet
sudo ufw allow from 192.168.1.0/24

## Allow a port range
sudo ufw allow 1000:2000/tcp

## Allow a service by name
sudo ufw allow OpenSSH

# ---------------------------------------------------------------
# 7. Auditing with auditd
# ---------------------------------------------------------------
# auditd logs security‑related events:
#   - file access
#   - permission changes
#   - sudo usage
#   - system calls

## Install auditd
sudo apt install auditd -y

## Check auditd status
sudo systemctl status auditd

## View audit logs
sudo ausearch -m USER_LOGIN

## Search for failed logins
sudo ausearch -m USER_LOGIN --success no

# ---------------------------------------------------------------
# 8. Creating Audit Rules
# ---------------------------------------------------------------

## Monitor a file for read/write/execute
sudo auditctl -w /etc/passwd -p rwxa -k passwd_changes

## Monitor a directory
sudo auditctl -w /var/www/ -p rwx -k web_changes

## View logs for a specific key
sudo ausearch -k passwd_changes

## Make rules persistent
sudo nano /etc/audit/rules.d/audit.rules

# ---------------------------------------------------------------
# 9. Security Hardening Essentials
# ---------------------------------------------------------------

## Disable root SSH login
sudo nano /etc/ssh/sshd_config
# Set:
# PermitRootLogin no

## Disable password SSH login
# PasswordAuthentication no

## Keep system updated
sudo apt update && sudo apt upgrade -y

## Remove unused packages
sudo apt autoremove

## Check for world-writable files
sudo find / -type f -perm -0002

## Check for SUID binaries
sudo find / -perm -4000

# ---------------------------------------------------------------
# 10. Practical Security Examples
# ---------------------------------------------------------------

## Restrict Nginx with AppArmor
sudo aa-enforce /etc/apparmor.d/usr.sbin.nginx

## Allow only SSH and HTTPS
sudo ufw allow 22/tcp
sudo ufw allow 443/tcp
sudo ufw default deny incoming

## Audit changes to /etc
sudo auditctl -w /etc -p wa -k etc_changes

## Monitor suspicious login attempts
sudo ausearch -m USER_LOGIN --success no

# ---------------------------------------------------------------
# 11. Summary
# ---------------------------------------------------------------
# - AppArmor → application sandboxing
# - enforce / complain → security modes
# - UFW → simple firewall management
# - auditd → track security events
# - Hardening → disable root login, limit ports, update system
#
# Next lesson:
# → Bash Scripting Basics (variables, loops, conditions)

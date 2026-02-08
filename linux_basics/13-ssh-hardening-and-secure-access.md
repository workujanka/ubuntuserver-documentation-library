# SSH Hardening & Secure Access
# -----------------------------
# This lesson explains how to secure SSH access on a Linux server.
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Why SSH Hardening Matters
# ---------------------------------------------------------------
# SSH is the primary way to access Linux servers remotely.
# Attackers constantly scan the internet for:
#   - weak passwords
#   - open SSH ports
#   - default configurations
#
# Hardening SSH reduces the risk of unauthorized access.

## Key Concepts
- SSH = secure remote access
- Public key = stored on server
- Private key = stored on your machine
- sshd = SSH daemon (server)

# ---------------------------------------------------------------
# 2. Checking SSH Status
# ---------------------------------------------------------------

## Check if SSH is running
systemctl status ssh

## Restart SSH
sudo systemctl restart ssh

## View SSH logs
sudo journalctl -u ssh -f

# ---------------------------------------------------------------
# 3. SSH Key Authentication (Recommended)
# ---------------------------------------------------------------

## Generate SSH key pair (on your local machine)
ssh-keygen -t ed25519 -C "workua"

## Copy public key to server
ssh-copy-id user@server-ip

## Manual method
cat ~/.ssh/id_ed25519.pub | ssh user@server-ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

## Test login
ssh user@server-ip

# ---------------------------------------------------------------
# 4. Disable Password Authentication
# ---------------------------------------------------------------
# After key authentication works, disable passwords.

## Edit SSH config
sudo nano /etc/ssh/sshd_config

## Set the following:
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM yes

## Restart SSH
sudo systemctl restart ssh

# ---------------------------------------------------------------
# 5. Change Default SSH Port (Optional)
# ---------------------------------------------------------------
# Default port 22 is heavily targeted by bots.

## Edit SSH config
sudo nano /etc/ssh/sshd_config

## Change:
Port 22
# to something like:
Port 2222

## Restart SSH
sudo systemctl restart ssh

## Update firewall
sudo ufw allow 2222/tcp

# ---------------------------------------------------------------
# 6. Disable Root Login
# ---------------------------------------------------------------

## Edit SSH config
sudo nano /etc/ssh/sshd_config

## Set:
PermitRootLogin no

## Restart SSH
sudo systemctl restart ssh

# ---------------------------------------------------------------
# 7. Limit SSH Access to Specific Users
# ---------------------------------------------------------------

## Edit SSH config
sudo nano /etc/ssh/sshd_config

## Add:
AllowUsers workua developer

## Restart SSH
sudo systemctl restart ssh

# ---------------------------------------------------------------
# 8. Use Fail2ban for Brute-Force Protection
# ---------------------------------------------------------------

## Install fail2ban
sudo apt install fail2ban -y

## Copy default config
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local

## Enable SSH protection
sudo nano /etc/fail2ban/jail.local

## Ensure:
[sshd]
enabled = true
port = ssh
logpath = /var/log/auth.log
maxretry = 5

## Restart fail2ban
sudo systemctl restart fail2ban

## Check status
sudo fail2ban-client status sshd

# ---------------------------------------------------------------
# 9. Use SSH Config File (Client Side)
# ---------------------------------------------------------------

## Edit local SSH config
nano ~/.ssh/config

## Example:
Host myserver
    HostName 192.168.1.10
    User workua
    Port 2222
    IdentityFile ~/.ssh/id_ed25519

## Connect easily
ssh myserver

# ---------------------------------------------------------------
# 10. Two-Factor Authentication (Optional)
# ---------------------------------------------------------------

## Install Google Authenticator
sudo apt install libpam-google-authenticator -y

## Run setup
google-authenticator

## Enable in PAM
sudo nano /etc/pam.d/sshd

## Add:
auth required pam_google_authenticator.so

## Enable in SSH config
sudo nano /etc/ssh/sshd_config

## Set:
ChallengeResponseAuthentication yes

## Restart SSH
sudo systemctl restart ssh

# ---------------------------------------------------------------
# 11. Practical Hardening Checklist
# ---------------------------------------------------------------

## ✔ Use SSH keys
## ✔ Disable password login
## ✔ Disable root login
## ✔ Change SSH port (optional)
## ✔ Install fail2ban
## ✔ Limit allowed users
## ✔ Use firewall rules
## ✔ Monitor logs

# ---------------------------------------------------------------
# 12. Summary
# ---------------------------------------------------------------
# - SSH keys → secure authentication
# - Disable passwords → prevents brute-force attacks
# - Disable root login → reduces risk
# - Change port → reduces noise from bots
# - Fail2ban → blocks repeated attackers
# - AllowUsers → restricts access
# - ~/.ssh/config → simplifies connections
#
# Next lesson:
# → System Backup & Restore (rsync, tar, snapshots)

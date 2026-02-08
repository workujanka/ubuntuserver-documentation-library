# systemctl & Services in Linux
# -----------------------------
# This lesson explains how to manage services using systemd and systemctl.
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. What Is systemd?
# ---------------------------------------------------------------
# systemd is the init system used by modern Linux distributions.
# It is responsible for:
#   - starting services at boot
#   - stopping services
#   - restarting services
#   - managing logs
#   - tracking service status
#
# systemctl is the command used to control systemd.

## Key Terms
- Service = a background program (e.g., ssh, nginx)
- Unit = a systemd-managed object (service, mount, timer)
- systemctl = tool to manage units

# ---------------------------------------------------------------
# 2. Checking Service Status
# ---------------------------------------------------------------

## Check if a service is running
systemctl status ssh
# Shows active/inactive, logs, PID, uptime

## Check only active state
systemctl is-active ssh

## Check if service is enabled at boot
systemctl is-enabled ssh

# ---------------------------------------------------------------
# 3. Starting, Stopping, Restarting Services
# ---------------------------------------------------------------

## Start a service
sudo systemctl start ssh

## Stop a service
sudo systemctl stop ssh

## Restart a service
sudo systemctl restart ssh

## Reload configuration without stopping
sudo systemctl reload ssh

# ---------------------------------------------------------------
# 4. Enabling & Disabling Services at Boot
# ---------------------------------------------------------------

## Enable service to start at boot
sudo systemctl enable ssh

## Disable service from starting at boot
sudo systemctl disable ssh

## Enable and start immediately
sudo systemctl enable --now ssh

## Disable and stop immediately
sudo systemctl disable --now ssh

# ---------------------------------------------------------------
# 5. Viewing All Services
# ---------------------------------------------------------------

## List all services
systemctl list-units --type=service

## List only running services
systemctl list-units --type=service --state=running

## List failed services
systemctl --failed

# ---------------------------------------------------------------
# 6. Logs for Services (journalctl)
# ---------------------------------------------------------------

## View logs for a specific service
sudo journalctl -u ssh

## Follow logs in real time
sudo journalctl -u ssh -f

## Show logs since boot
sudo journalctl -b

## Show logs for last 1 hour
sudo journalctl --since "1 hour ago"

# ---------------------------------------------------------------
# 7. Service Unit Files
# ---------------------------------------------------------------
# Unit files define how services run.
# They are stored in:
#   /etc/systemd/system/      → custom services
#   /lib/systemd/system/      → default services

## View a unit file
cat /lib/systemd/system/ssh.service

## Reload systemd after editing a unit file
sudo systemctl daemon-reload

# ---------------------------------------------------------------
# 8. Creating a Custom Service
# ---------------------------------------------------------------
# Example: run a script as a service.

## 1. Create a script
# /usr/local/bin/myscript.sh

## 2. Make it executable
sudo chmod +x /usr/local/bin/myscript.sh

## 3. Create a service file
# /etc/systemd/system/myscript.service

## Example content:
# [Unit]
# Description=My Custom Script
#
# [Service]
# ExecStart=/usr/local/bin/myscript.sh
#
# [Install]
# WantedBy=multi-user.target

## 4. Reload systemd
sudo systemctl daemon-reload

## 5. Start the service
sudo systemctl start myscript

## 6. Enable at boot
sudo systemctl enable myscript

# ---------------------------------------------------------------
# 9. Troubleshooting Services
# ---------------------------------------------------------------

## Check service logs
sudo journalctl -u servicename -f

## Check if service is enabled
systemctl is-enabled servicename

## Check if service is running
systemctl is-active servicename

## Restart the service
sudo systemctl restart servicename

## Reload systemd
sudo systemctl daemon-reload

# ---------------------------------------------------------------
# 10. Summary
# ---------------------------------------------------------------
# - systemctl status → check service status
# - systemctl start/stop/restart → control services
# - systemctl enable/disable → manage boot behavior
# - journalctl -u → view service logs
# - daemon-reload → reload systemd after changes
#
# Next lesson:
# → Package Management (apt)

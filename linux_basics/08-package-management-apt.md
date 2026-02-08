# Package Management with APT
# ---------------------------
# This lesson explains how to install, update, remove, and search for software
# using APT (Advanced Package Tool) on Ubuntu. All commands include # comments.

# ---------------------------------------------------------------
# 1. What Is APT?
# ---------------------------------------------------------------
# APT is the package manager used by Debian and Ubuntu.
# It allows you to:
#   - install software
#   - update software
#   - remove software
#   - upgrade the entire system
#
# APT works with online repositories that contain thousands of packages.

## Key Concepts
- Package = a piece of software (nginx, curl, git)
- Repository = online storage for packages
- apt = user-friendly command
- dpkg = low-level package tool

# ---------------------------------------------------------------
# 2. Updating Package Lists
# ---------------------------------------------------------------

## Update the list of available packages
sudo apt update
# This does NOT upgrade anything.
# It only refreshes the list of what is available.

## Check if upgrades are available
apt list --upgradable

# ---------------------------------------------------------------
# 3. Upgrading Packages
# ---------------------------------------------------------------

## Upgrade installed packages
sudo apt upgrade
# Installs newer versions of installed software.

## Full upgrade (handles dependencies)
sudo apt full-upgrade
# May remove or replace packages if needed.

## Upgrade everything automatically
sudo apt update && sudo apt upgrade -y

# ---------------------------------------------------------------
# 4. Installing Software
# ---------------------------------------------------------------

## Install a package
sudo apt install nginx

## Install multiple packages
sudo apt install curl git htop

## Install a specific version
sudo apt install nginx=1.18.0-0ubuntu1

## Reinstall a package
sudo apt install --reinstall nginx

# ---------------------------------------------------------------
# 5. Removing Software
# ---------------------------------------------------------------

## Remove a package (keep config files)
sudo apt remove nginx

## Remove package + config files
sudo apt purge nginx

## Remove unused dependencies
sudo apt autoremove

# ---------------------------------------------------------------
# 6. Searching for Packages
# ---------------------------------------------------------------

## Search by name
apt search nginx

## Show detailed info
apt show nginx
# Includes:
#   - version
#   - description
#   - dependencies
#   - maintainer

# ---------------------------------------------------------------
# 7. Managing .deb Files (dpkg)
# ---------------------------------------------------------------
# dpkg installs local .deb files (not from repositories).

## Install a .deb file
sudo dpkg -i package.deb

## Fix missing dependencies
sudo apt --fix-broken install

## List installed packages
dpkg -l

## Check if a package is installed
dpkg -l | grep nginx

# ---------------------------------------------------------------
# 8. Cleaning the Package Cache
# ---------------------------------------------------------------

## Clear downloaded package files
sudo apt clean

## Clear only outdated package files
sudo apt autoclean

# ---------------------------------------------------------------
# 9. Checking Package Sources
# ---------------------------------------------------------------

## List repositories
cat /etc/apt/sources.list

## List additional sources
ls /etc/apt/sources.list.d/

# ---------------------------------------------------------------
# 10. Practical Troubleshooting
# ---------------------------------------------------------------

## Fix broken installs
sudo apt --fix-broken install

## Fix dpkg lock issues
sudo rm /var/lib/dpkg/lock-frontend
sudo dpkg --configure -a

## Fix partial installs
sudo apt install -f

# ---------------------------------------------------------------
# 11. Summary
# ---------------------------------------------------------------
# - apt update → refresh package list
# - apt upgrade → upgrade installed packages
# - apt install → install software
# - apt remove / purge → uninstall software
# - apt autoremove → clean unused dependencies
# - apt show → package details
# - dpkg -i → install .deb files
#
# Next lesson:
# → File Permissions & Ownership (Advanced)

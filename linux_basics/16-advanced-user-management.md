# Advanced User Management (sudoers, ACLs)
# ----------------------------------------
# This lesson covers advanced Linux user management:
#   - sudoers file
#   - privilege delegation
#   - access control lists (ACLs)
#   - secure permission management
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Understanding sudo and sudoers
# ---------------------------------------------------------------
# sudo allows a user to run commands as another user (usually root).
# The sudoers file controls:
#   - who can run sudo
#   - which commands they can run
#   - logging and security rules

## Check if a user has sudo access
sudo -l

## Add a user to the sudo group
sudo usermod -aG sudo username

# ---------------------------------------------------------------
# 2. Editing the sudoers File (SAFE METHOD)
# ---------------------------------------------------------------
# NEVER edit /etc/sudoers directly.
# ALWAYS use visudo — it checks for syntax errors.

## Open sudoers safely
sudo visudo

## Common entries:
# Allow a user full sudo access:
#   username ALL=(ALL:ALL) ALL
#
# Allow a group full sudo access:
#   %admin ALL=(ALL) ALL
#
# Allow a user to run only specific commands:
#   username ALL=(ALL) /usr/bin/systemctl restart nginx
#
# Allow passwordless sudo:
#   username ALL=(ALL) NOPASSWD: ALL

# ---------------------------------------------------------------
# 3. sudoers.d Directory (Modular Configuration)
# ---------------------------------------------------------------
# Instead of editing the main sudoers file, create separate files.

## Create a custom sudo rule
sudo nano /etc/sudoers.d/deploy

## Example content:
# deploy ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx

## Validate syntax
sudo visudo -c

# ---------------------------------------------------------------
# 4. Access Control Lists (ACLs)
# ---------------------------------------------------------------
# ACLs allow fine‑grained permissions beyond the basic rwx model.
# Useful when multiple users need different access to the same file.

## Check if ACL support is installed
sudo apt install acl -y

# ---------------------------------------------------------------
# 5. Viewing ACLs
# ---------------------------------------------------------------

## Show ACLs for a file
getfacl file.txt

## Example output:
# user:workua:rwx
# group:developers:rw-
# other::r--

# ---------------------------------------------------------------
# 6. Setting ACLs
# ---------------------------------------------------------------

## Give a user read/write access
setfacl -m u:username:rw file.txt

## Give a group read/write/execute access
setfacl -m g:developers:rwx /project

## Set default ACLs for new files in a directory
setfacl -d -m g:developers:rwx /project

## Remove ACL entry
setfacl -x u:username file.txt

# ---------------------------------------------------------------
# 7. Recursive ACLs
# ---------------------------------------------------------------

## Apply ACLs to all files and subfolders
setfacl -R -m g:developers:rwX /project
# X = execute only if directory or already executable

# ---------------------------------------------------------------
# 8. Combining ACLs with Traditional Permissions
# ---------------------------------------------------------------
# ACLs do NOT replace chmod — they extend it.
# If both exist, ACLs override chmod for specific users/groups.

## Example:
chmod 770 /project
setfacl -m u:tester:r /project

# tester now has read access even though "others" have none.

# ---------------------------------------------------------------
# 9. Removing All ACLs
# ---------------------------------------------------------------

## Remove all ACLs from a file
setfacl -b file.txt

## Remove all ACLs recursively
setfacl -Rb /project

# ---------------------------------------------------------------
# 10. Practical Examples
# ---------------------------------------------------------------

## Allow developer to restart nginx without password
sudo nano /etc/sudoers.d/devops
# devops ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx

## Give QA team read‑only access to logs
setfacl -m g:qa:r /var/log/app/

## Give intern access to one script only
setfacl -m u:intern:rx /usr/local/bin/report.sh

## Create shared project folder
mkdir /project
chgrp developers /project
chmod 2770 /project        # 2 = setgid bit
setfacl -m g:developers:rwx /project

# ---------------------------------------------------------------
# 11. Troubleshooting
# ---------------------------------------------------------------

## Check effective permissions
namei -l /path/to/file

## Check sudo syntax
sudo visudo -c

## Check ACL conflicts
getfacl file.txt

## Fix broken sudoers file (if locked out)
pkexec visudo

# ---------------------------------------------------------------
# 12. Summary
# ---------------------------------------------------------------
# - sudoers controls privileged access
# - visudo ensures safe editing
# - sudoers.d allows modular rules
# - ACLs provide fine‑grained permissions
# - setfacl / getfacl manage ACLs
# - chmod + ACLs work together
#
# Next lesson:
# → Networking Tools (curl, wget, dig, traceroute)

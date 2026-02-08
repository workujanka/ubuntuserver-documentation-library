# Users & Groups in Linux
# -----------------------
# This lesson explains how Linux handles users, groups, permissions,
# and account management. All commands include # comments for clarity.

# -----------------------------------------------
# 1. Why Users & Groups Matter
# -----------------------------------------------
# Linux is a multi-user system.
# Every file, folder, and process belongs to:
#   - a user
#   - a group
#   - an owner
#
# Permissions (r, w, x) depend on these two.

## Key Concepts
- User = an account (e.g., workua2)
- Group = a collection of users
- Root(owner)= the superuser with full control

# -----------------------------------------------
# 2. Viewing User Information
# -----------------------------------------------

## Show current user
whoami        # prints your username

## Show all logged-in users
who           # shows all active sessions

## Show user details
id            # shows UID, GID, and groups

# -----------------------------------------------
# 3. Managing Users
# -----------------------------------------------

## Create a new user
sudo adduser username     # creates user + home folder + password

## Delete a user
sudo deluser username                 # removes user but keeps home folder
sudo deluser --remove-home username   # removes user + home folder

## Change a user’s password
sudo passwd username

# -----------------------------------------------
# 4. Managing Groups
# -----------------------------------------------

## Create a group
sudo addgroup groupname

## Delete a group
sudo delgroup groupname

## Add a user to a group
sudo usermod -aG groupname username   # -aG = append to group (important!)

## Remove a user from a group
sudo gpasswd -d username groupname

## Show all groups
getent group

# -----------------------------------------------
# 5. Understanding /etc/passwd and /etc/group
# -----------------------------------------------

## View user database
cat /etc/passwd
# Format:
# username:x:UID:GID:comment:home:shell

## View group database
cat /etc/group
# Format:
# groupname:x:GID:user1,user2

# -----------------------------------------------
# 6. Switching Users
# -----------------------------------------------

## Switch to another user
su - username

## Switch to root (if allowed)
sudo -i

# -----------------------------------------------
# 7. Sudo Access
# -----------------------------------------------

## Add user to sudo group
sudo usermod -aG sudo username

## Test sudo access
sudo ls /root

# -----------------------------------------------
# 8. Practical Examples
# -----------------------------------------------

## Create a developer user
sudo adduser developer
sudo usermod -aG sudo developer

## Create a group for web admins
sudo addgroup webadmins
sudo usermod -aG webadmins developer

## Check the result
id developer

# -----------------------------------------------
# 9. Summary
# -----------------------------------------------
# - Users own files and run processes
# - Groups organize users
# - Permissions depend on user + group
# - sudo gives admin privileges
# - /etc/passwd and /etc/group store account info

# This lesson prepares you for the next topic:
# → Networking Basics

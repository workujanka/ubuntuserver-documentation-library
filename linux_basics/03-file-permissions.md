# File Permissions in Linux
# -------------------------
# This lesson explains how Linux permissions work, how to read them,
# and how to change them using chmod and chown.
# All comments starting with "#" are explanations for beginners.

# ---------------------------------------------------------------
# 1. Permission Types
# ---------------------------------------------------------------
# Every file and folder in Linux has three types of permissions:
# r = read    → view file contents or list directory
# w = write   → modify file contents or create/delete files in a directory
# x = execute → run a file as a program or enter a directory

## Permission Types
- Read (r)
- Write (w)
- Execute (x)

# ---------------------------------------------------------------
# 2. Permission Groups
# ---------------------------------------------------------------
# Permissions apply to three groups:
# owner → the user who owns the file
# group → users who belong to the file's group
# others → everyone else on the system

## Permission Groups
- Owner
- Group
- Others

# ---------------------------------------------------------------
# 3. Viewing Permissions
# ---------------------------------------------------------------
# Use "ls -l" to see permissions.
# Example output:
# -rwxr-xr--  1 user group  4096 Feb 8  file.sh
#
# Breakdown:
# -          → file type
# rwx        → owner permissions
# r-x        → group permissions
# r--        → others permissions


# ---------------------------------------------------------------
# 4. Changing Permissions (chmod)
# ---------------------------------------------------------------
# chmod changes file permissions.
# Two common methods:
#
# A) Numeric (e.g., 755)
#    7 = rwx
#    5 = r-x
#    5 = r-x
#
# B) Symbolic (e.g., u+x)
#    u = user/owner
#    g = group
#    o = others
#    a = all

## Changing permissions e.g.

chmod 755 file.sh         # owner: rwx, group: r-x, others: r-x
chmod u+x script.sh       # add execute permission for owner
chmod g-w file.txt        # remove write permission from group


# ---------------------------------------------------------------
# 5. Changing Ownership (chown)
# ---------------------------------------------------------------
# chown changes the owner and/or group of a file.
# Only root or sudo users can do this.

## Changing ownership e.g.

sudo chown user:group file.txt    # change owner and group
sudo chown workua file.txt       # change only owner
sudo chown :developers file.txt   # change only group
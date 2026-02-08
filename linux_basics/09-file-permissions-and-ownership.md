# File Permissions & Ownership (Advanced)
# ---------------------------------------
# This lesson explains Linux file permissions, ownership, symbolic modes,
# numeric modes, and how to manage access securely. All commands include # comments.

# ---------------------------------------------------------------
# 1. Why Permissions Matter
# ---------------------------------------------------------------
# Linux uses permissions to control who can:
#   - read files
#   - modify files
#   - execute programs
#
# Every file and directory has:
#   - owner (user)
#   - group
#   - permissions for user, group, others

## Example permission string:
# -rwxr-x---
# | |  |  |
# | |  |  └── others permissions
# | |  └──── group permissions
# | └─────── owner permissions
# └────────── file type (- = file, d = directory)

# ---------------------------------------------------------------
# 2. Viewing Permissions
# ---------------------------------------------------------------

## List files with permissions
ls -l

## Example output:
# -rw-r--r-- 1 workua workua 1200 Feb 10 notes.txt
# |  |  |     |      |
# |  |  |     |      └── group
# |  |  |     └──────── user
# |  |  └────────────── number of links
# |  └────────────────── permissions
# └───────────────────── file type

# ---------------------------------------------------------------
# 3. Understanding Permission Types
# ---------------------------------------------------------------

## Read (r)
# Files: view contents
# Directories: list files

## Write (w)
# Files: modify contents
# Directories: create/delete files

## Execute (x)
# Files: run as program
# Directories: enter the directory (cd)

# ---------------------------------------------------------------
# 4. Changing Permissions (chmod)
# ---------------------------------------------------------------

## Symbolic mode
chmod u+r file.txt      # add read for user
chmod g-w file.txt      # remove write for group
chmod o+x script.sh     # add execute for others
chmod u=rwx,g=rx,o= file.txt   # set exact permissions

## Numeric mode
# r = 4, w = 2, x = 1
# Add them together:
# rwx = 7, rw- = 6, r-- = 4

chmod 755 script.sh     # rwx r-x r-x
chmod 644 file.txt      # rw- r-- r--

# ---------------------------------------------------------------
# 5. Changing Ownership (chown)
# ---------------------------------------------------------------

## Change owner
sudo chown newuser file.txt

## Change owner and group
sudo chown newuser:newgroup file.txt

## Change ownership recursively
sudo chown -R newuser:newgroup /var/www/

# ---------------------------------------------------------------
# 6. Changing Group Ownership (chgrp)
# ---------------------------------------------------------------

## Change group only
sudo chgrp developers file.txt

## Recursive
sudo chgrp -R developers /project/

# ---------------------------------------------------------------
# 7. Special Permissions (Advanced)
# ---------------------------------------------------------------

## Setuid (s)
# When set on a file, it runs with the owner's permissions.
chmod u+s program

## Setgid (s)
# When set on a directory, new files inherit the directory's group.
chmod g+s shared_folder

## Sticky bit (t)
# Only the file owner can delete files in the directory.
chmod +t /tmp

## Example:
# drwxrwxrwt  → sticky bit set (common in /tmp)

# ---------------------------------------------------------------
# 8. Practical Examples
# ---------------------------------------------------------------

## Make a script executable
chmod +x backup.sh

## Secure a private file
chmod 600 secrets.txt    # rw------- (only owner)

## Share a folder with a group
sudo chgrp developers /project
chmod 770 /project       # rwx for user & group

## Make a shared folder inherit group
chmod g+s /project

# ---------------------------------------------------------------
# 9. Permission Troubleshooting
# ---------------------------------------------------------------

## Check who owns a file
ls -l file.txt

## Check effective permissions
namei -l /path/to/file

## Fix permission denied errors
sudo chown -R $USER:$USER folder
chmod -R 755 folder

# ---------------------------------------------------------------
# 10. Summary
# ---------------------------------------------------------------
# - ls -l → view permissions
# - chmod → change permissions (symbolic or numeric)
# - chown → change owner/group
# - chgrp → change group only
# - setuid/setgid/sticky → advanced access control
# - 755, 644, 600 → common permission patterns
#
# Next lesson:
# → Disk Management & Filesystems

# Disk Management & Filesystems
# -----------------------------
# This lesson explains how Linux handles disks, partitions, filesystems,
# mounting, unmounting, and monitoring storage. All commands include # comments.

# ---------------------------------------------------------------
# 1. Understanding Disks, Partitions, and Filesystems
# ---------------------------------------------------------------
# Disk = physical storage device (e.g., /dev/sda)
# Partition = a section of a disk (e.g., /dev/sda1)
# Filesystem = structure used to store files (ext4, xfs, ntfs)
# Mount point = directory where a filesystem becomes accessible

## Common Linux device names:
# /dev/sda   → first disk
# /dev/sdb   → second disk
# /dev/nvme0n1 → NVMe SSD
# /dev/mapper/… → LVM volumes

# ---------------------------------------------------------------
# 2. Viewing Disks and Partitions
# ---------------------------------------------------------------

## List all disks and partitions
lsblk
# Shows NAME, SIZE, TYPE, MOUNTPOINT

## Detailed disk info
sudo fdisk -l

## Show filesystem types
lsblk -f

# ---------------------------------------------------------------
# 3. Checking Disk Usage
# ---------------------------------------------------------------

## Human-readable disk usage
df -h

## Check inode usage
df -i

## Check directory size
du -sh /var/log

## Show size of all folders in current directory
du -sh *

# ---------------------------------------------------------------
# 4. Creating Partitions (fdisk)
# ---------------------------------------------------------------
# WARNING: fdisk modifies disks. Use carefully.

## Open disk for partitioning
sudo fdisk /dev/sdb

## Inside fdisk:
# n → new partition
# d → delete partition
# p → print table
# w → write changes
# q → quit without saving

# ---------------------------------------------------------------
# 5. Creating Filesystems
# ---------------------------------------------------------------

## Create ext4 filesystem
sudo mkfs.ext4 /dev/sdb1

## Create xfs filesystem
sudo mkfs.xfs /dev/sdb1

## Create swap partition
sudo mkswap /dev/sdb2

## Activate swap
sudo swapon /dev/sdb2

# ---------------------------------------------------------------
# 6. Mounting and Unmounting
# ---------------------------------------------------------------

## Create mount point
sudo mkdir /mnt/data

## Mount a filesystem
sudo mount /dev/sdb1 /mnt/data

## Unmount
sudo umount /mnt/data

## Check mounted filesystems
mount | grep sdb1

# ---------------------------------------------------------------
# 7. Permanent Mounts (fstab)
# ---------------------------------------------------------------
# /etc/fstab defines filesystems that mount at boot.

## View fstab
cat /etc/fstab

## Example entry:
# /dev/sdb1   /mnt/data   ext4   defaults   0   2

## Test fstab without reboot
sudo mount -a

# ---------------------------------------------------------------
# 8. Checking Disk Health (SMART)
# ---------------------------------------------------------------

## Install smartmontools
sudo apt install smartmontools -y

## Check disk health
sudo smartctl -H /dev/sda

## Detailed report
sudo smartctl -a /dev/sda

# ---------------------------------------------------------------
# 9. LVM (Logical Volume Manager) Basics
# ---------------------------------------------------------------
# LVM allows flexible disk management:
#   - resize volumes
#   - combine disks
#   - create snapshots

## Show LVM volumes
sudo lvdisplay

## Show volume groups
sudo vgdisplay

## Show physical volumes
sudo pvdisplay

# ---------------------------------------------------------------
# 10. Resizing Filesystems (Advanced)
# ---------------------------------------------------------------

## Resize ext4 after expanding partition
sudo resize2fs /dev/sdb1

## Resize xfs (must be mounted)
sudo xfs_growfs /mnt/data

# ---------------------------------------------------------------
# 11. Practical Examples
# ---------------------------------------------------------------

## Format and mount a new disk
sudo mkfs.ext4 /dev/sdb1
sudo mkdir /data
sudo mount /dev/sdb1 /data

## Add to fstab
# /dev/sdb1   /data   ext4   defaults   0   2

## Check disk usage
df -h

## Check largest folders
du -sh /*

# ---------------------------------------------------------------
# 12. Summary
# ---------------------------------------------------------------
# - lsblk → view disks
# - df -h → disk usage
# - du -sh → folder sizes
# - fdisk → partitioning
# - mkfs → create filesystem
# - mount / umount → attach/detach disks
# - /etc/fstab → permanent mounts
# - smartctl → disk health
# - LVM → flexible storage management
#
# Next lesson:
# → Networking Services & Ports (Advanced)

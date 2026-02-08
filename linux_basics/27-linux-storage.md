# Linux Storage (LVM, RAID, Partitions, Filesystems)
# --------------------------------------------------
# This lesson covers advanced storage management:
#   - partitions
#   - filesystems
#   - LVM (Logical Volume Manager)
#   - RAID (software RAID)
#   - mounting and resizing
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Disk & Partition Basics
# ---------------------------------------------------------------

## List disks
lsblk

## Show detailed disk info
sudo fdisk -l

## Create or modify partitions
sudo fdisk /dev/sda

## Common partition types:
# 83 → Linux filesystem
# 8e → LVM
# fd → RAID autodetect

# ---------------------------------------------------------------
# 2. Filesystems
# ---------------------------------------------------------------

## Create ext4 filesystem
sudo mkfs.ext4 /dev/sda1

## Create XFS filesystem
sudo mkfs.xfs /dev/sdb1

## Check filesystem type
lsblk -f

## Mount filesystem
sudo mount /dev/sda1 /mnt

## Unmount
sudo umount /mnt

## Make mount persistent
sudo nano /etc/fstab
# Example:
# /dev/sda1   /mnt   ext4   defaults   0 2

# ---------------------------------------------------------------
# 3. LVM Overview
# ---------------------------------------------------------------
# LVM provides flexible storage management:
#   - PV (Physical Volume)
#   - VG (Volume Group)
#   - LV (Logical Volume)

## Install LVM tools
sudo apt install lvm2 -y

# ---------------------------------------------------------------
# 4. Creating LVM Storage
# ---------------------------------------------------------------

## Create physical volume
sudo pvcreate /dev/sdb

## Create volume group
sudo vgcreate vgdata /dev/sdb

## Create logical volume (10GB)
sudo lvcreate -L 10G -n lvbackup vgdata

## Create filesystem on LV
sudo mkfs.ext4 /dev/vgdata/lvbackup

## Mount LV
sudo mount /dev/vgdata/lvbackup /backup

# ---------------------------------------------------------------
# 5. Extending LVM Volumes
# ---------------------------------------------------------------

## Extend LV by 5GB
sudo lvextend -L +5G /dev/vgdata/lvbackup

## Resize filesystem (ext4)
sudo resize2fs /dev/vgdata/lvbackup

## Resize filesystem (XFS)
sudo xfs_growfs /backup

# ---------------------------------------------------------------
# 6. Reducing LVM Volumes (ext4 only)
# ---------------------------------------------------------------

## Unmount first
sudo umount /backup

## Check filesystem
sudo e2fsck -f /dev/vgdata/lvbackup

## Shrink filesystem
sudo resize2fs /dev/vgdata/lvbackup 8G

## Reduce LV
sudo lvreduce -L 8G /dev/vgdata/lvbackup

## Remount
sudo mount /dev/vgdata/lvbackup /backup

# ---------------------------------------------------------------
# 7. Adding More Disks to LVM
# ---------------------------------------------------------------

## Add new disk as PV
sudo pvcreate /dev/sdc

## Extend VG
sudo vgextend vgdata /dev/sdc

## Extend LV to use all free space
sudo lvextend -l +100%FREE /dev/vgdata/lvbackup

## Resize filesystem
sudo resize2fs /dev/vgdata/lvbackup

# ---------------------------------------------------------------
# 8. Software RAID (mdadm)
# ---------------------------------------------------------------

## Install mdadm
sudo apt install mdadm -y

## Create RAID 1 (mirroring)
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc

## Create RAID 5
sudo mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sd[bcd]

## Check RAID status
cat /proc/mdstat

## Save RAID config
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf

# ---------------------------------------------------------------
# 9. Using RAID with LVM
# ---------------------------------------------------------------

## Create PV on RAID
sudo pvcreate /dev/md0

## Create VG
sudo vgcreate vgraid /dev/md0

## Create LV
sudo lvcreate -L 20G -n lvdata vgraid

## Create filesystem
sudo mkfs.ext4 /dev/vgraid/lvdata

# ---------------------------------------------------------------
# 10. Checking Disk Health (SMART)
# ---------------------------------------------------------------

## Install smart tools
sudo apt install smartmontools -y

## Check disk health
sudo smartctl -a /dev/sda

## Run short test
sudo smartctl -t short /dev/sda

# ---------------------------------------------------------------
# 11. Disk Usage & Monitoring
# ---------------------------------------------------------------

## Check disk usage
df -h

## Check inode usage
df -i

## Check folder sizes
du -sh /var/*

# ---------------------------------------------------------------
# 12. Practical Examples
# ---------------------------------------------------------------

## 1. Create LVM storage for Docker
sudo pvcreate /dev/sdb
sudo vgcreate vgdocker /dev/sdb
sudo lvcreate -L 50G -n lvdocker vgdocker
sudo mkfs.ext4 /dev/vgdocker/lvdocker

## 2. Create RAID1 for critical data
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdc /dev/sdd

## 3. Extend root filesystem (LVM)
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv

# ---------------------------------------------------------------
# 13. Summary
# ---------------------------------------------------------------
# - lsblk / fdisk → inspect disks
# - mkfs → create filesystems
# - LVM → flexible storage (PV, VG, LV)
# - lvextend / resize2fs → grow storage
# - mdadm → software RAID
# - smartctl → disk health
# - /etc/fstab → persistent mounts
#
# Next lesson:
# → Linux Containers (Docker, Podman, images, volumes)

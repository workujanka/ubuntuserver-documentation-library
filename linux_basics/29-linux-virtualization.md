# Linux Virtualization (KVM, QEMU, virt‑manager)
# ----------------------------------------------
# This lesson covers full virtualization on Linux using:
#   - KVM (Kernel-based Virtual Machine)
#   - QEMU (hardware emulator)
#   - virt‑manager (GUI management)
#   - bridges, storage pools, snapshots
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. What Is KVM?
# ---------------------------------------------------------------
# KVM turns the Linux kernel into a hypervisor.
# It provides:
#   - near-native performance
#   - hardware virtualization (Intel VT-x / AMD-V)
#   - support for Windows, Linux, BSD VMs

## Check if CPU supports virtualization
egrep -c '(vmx|svm)' /proc/cpuinfo

## Check if KVM modules are loaded
lsmod | grep kvm

# ---------------------------------------------------------------
# 2. Install KVM, QEMU, virt‑manager
# ---------------------------------------------------------------

sudo apt update
sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager

## Enable and start libvirt
sudo systemctl enable libvirtd
sudo systemctl start libvirtd

## Add user to libvirt group
sudo usermod -aG libvirt $USER

## Add user to kvm group
sudo usermod -aG kvm $USER

# ---------------------------------------------------------------
# 3. Verify Installation
# ---------------------------------------------------------------

## Check libvirt service
systemctl status libvirtd

## Check virtualization capabilities
virsh capabilities

## List available hypervisors
virsh list --all

# ---------------------------------------------------------------
# 4. Creating a VM (CLI with virt-install)
# ---------------------------------------------------------------

## Example: Create Ubuntu VM
sudo virt-install \
  --name ubuntu-vm \
  --ram 4096 \
  --vcpus 2 \
  --disk size=20 \
  --os-variant ubuntu22.04 \
  --cdrom /iso/ubuntu.iso \
  --network network=default

# ---------------------------------------------------------------
# 5. Managing VMs with virsh
# ---------------------------------------------------------------

## List running VMs
virsh list

## List all VMs
virsh list --all

## Start VM
virsh start ubuntu-vm

## Shutdown VM
virsh shutdown ubuntu-vm

## Force stop
virsh destroy ubuntu-vm

## Autostart VM on boot
virsh autostart ubuntu-vm

## Delete VM
virsh undefine ubuntu-vm
virsh destroy ubuntu-vm

# ---------------------------------------------------------------
# 6. virt‑manager (GUI)
# ---------------------------------------------------------------
# virt‑manager provides a graphical interface for:
#   - creating VMs
#   - managing storage pools
#   - configuring networks
#   - snapshots

## Launch virt‑manager
virt-manager

## Common tasks:
# - Create VM → choose ISO → set CPU/RAM → create disk
# - Manage snapshots
# - Attach additional disks
# - Configure bridged networking

# ---------------------------------------------------------------
# 7. Storage Pools & Volumes
# ---------------------------------------------------------------

## List storage pools
virsh pool-list

## Create storage pool
virsh pool-define-as default dir - - - - "/var/lib/libvirt/images"

## Start pool
virsh pool-start default

## Autostart pool
virsh pool-autostart default

## Create volume
virsh vol-create-as default ubuntu.qcow2 20G --format qcow2

# ---------------------------------------------------------------
# 8. Snapshots
# ---------------------------------------------------------------

## Create snapshot
virsh snapshot-create-as ubuntu-vm snap1 "Before update"

## List snapshots
virsh snapshot-list ubuntu-vm

## Revert to snapshot
virsh snapshot-revert ubuntu-vm snap1

## Delete snapshot
virsh snapshot-delete ubuntu-vm snap1

# ---------------------------------------------------------------
# 9. Networking (NAT, Bridge)
# ---------------------------------------------------------------

## Default NAT network
virsh net-list

## Restart default network
virsh net-destroy default
virsh net-start default

## Create bridged network (Netplan example)
# network:
#   bridges:
#     br0:
#       interfaces: [eth0]
#       dhcp4: yes

## Attach VM to bridge
virsh attach-interface ubuntu-vm bridge br0 --model virtio --config

# ---------------------------------------------------------------
# 10. QEMU Standalone Usage
# ---------------------------------------------------------------

## Run VM directly with QEMU
qemu-system-x86_64 \
  -m 4G \
  -smp 2 \
  -hda ubuntu.qcow2 \
  -cdrom ubuntu.iso \
  -boot d

## Convert disk formats
qemu-img convert -O qcow2 disk.img disk.qcow2

## Resize disk
qemu-img resize ubuntu.qcow2 +10G

# ---------------------------------------------------------------
# 11. Cloud Images & Cloud-Init
# ---------------------------------------------------------------

## Download cloud image
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img

## Create cloud-init ISO
cloud-localds seed.iso user-data.yaml

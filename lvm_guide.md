# LVM (Logical Volume Manager) Complete Guide

## Table of Contents
1. [What is LVM?](#what-is-lvm)
2. [LVM Components](#lvm-components)
3. [Benefits of LVM](#benefits-of-lvm)
4. [Step-by-Step Implementation](#step-by-step-implementation)
5. [Common LVM Commands](#common-lvm-commands)
6. [Practical Examples](#practical-examples)
7. [Best Practices](#best-practices)
8. [Troubleshooting](#troubleshooting)

## What is LVM?

LVM (Logical Volume Manager) is a device mapper framework that provides logical volume management for the Linux kernel. It allows you to create flexible disk storage by abstracting the underlying physical storage devices.

## LVM Components

### Physical Volumes (PV)
- The actual hard disks or partitions
- Building blocks of LVM
- Must be initialized before use

### Volume Groups (VG)
- Collection of Physical Volumes
- Pool of storage from which Logical Volumes are created
- Can span multiple physical disks

### Logical Volumes (LV)
- Virtual partitions created from Volume Groups
- Can be resized dynamically
- Where filesystems are created

```
┌─────────────────────────────────────┐
│           Logical Volumes           │
│  ┌─────────┐  ┌─────────┐  ┌──────┐ │
│  │   LV1   │  │   LV2   │  │ LV3  │ │
│  └─────────┘  └─────────┘  └──────┘ │
├─────────────────────────────────────┤
│            Volume Group             │
│              (VG00)                 │
├─────────────────────────────────────┤
│         Physical Volumes            │
│  ┌─────────┐  ┌─────────┐  ┌──────┐ │
│  │ /dev/   │  │ /dev/   │  │/dev/ │ │
│  │ sdb1    │  │ sdc     │  │sdd1  │ │
│  └─────────┘  └─────────┘  └──────┘ │
└─────────────────────────────────────┘
```

## Benefits of LVM

- **Flexibility**: Resize volumes without unmounting
- **Snapshots**: Create point-in-time copies
- **Multiple Disks**: Combine multiple disks into single logical volumes
- **Dynamic Management**: Add/remove storage on the fly
- **Better Space Utilization**: Allocate space as needed

## Step-by-Step Implementation

### Step 1: Install New Hard Disk Drive

First, physically install the new hard disk drive and verify it's detected:

```bash
# List all block devices
lsblk

# Check disk information
fdisk -l
```

### Step 2: Create Partitions (Optional)

You can use entire disks or create partitions:

```bash
# Create partition using fdisk
fdisk /dev/sdb

# Or use parted for GPT
parted /dev/sdb mklabel gpt
parted /dev/sdb mkpart primary 0% 100%
```

### Step 3: Create Physical Volumes (PV)

Initialize disks/partitions as Physical Volumes:

```bash
# Create PV from partition
pvcreate /dev/sdb1

# Create PV from entire disk
pvcreate /dev/sdc

# Create multiple PVs at once
pvcreate /dev/sdd1 /dev/sde1
```

**Verify PV creation:**
```bash
# Display all PVs
pvdisplay

# Show PV summary
pvs

# Show specific PV
pvdisplay /dev/sdb1
```

### Step 4: Create Volume Group (VG)

Combine Physical Volumes into a Volume Group:

```bash
# Create VG with single PV
vgcreate vg00 /dev/sdb1

# Create VG with multiple PVs
vgcreate vg00 /dev/sdb1 /dev/sdc /dev/sdd1

# Extend existing VG
vgextend vg00 /dev/sde1
```

**Verify VG creation:**
```bash
# Display all VGs
vgdisplay

# Show VG summary
vgs

# Show specific VG
vgdisplay vg00
```

### Step 5: Create Logical Volumes (LV)

Create Logical Volumes from Volume Group space:

```bash
# Create LV with specific size
lvcreate -L 10G -n lv_data vg00

# Create LV using percentage of VG
lvcreate -l 50%VG -n lv_backup vg00

# Create LV using all remaining space
lvcreate -l 100%FREE -n lv_storage vg00
```

**Verify LV creation:**
```bash
# Display all LVs
lvdisplay

# Show LV summary
lvs

# Show specific LV
lvdisplay /dev/vg00/lv_data
```

### Step 6: Create Filesystem

Format the Logical Volume with desired filesystem:

```bash
# Create ext4 filesystem
mkfs.ext4 /dev/vg00/lv_data

# Create xfs filesystem
mkfs.xfs /dev/vg00/lv_backup

# Create filesystem with label
mkfs.ext4 -L "DATA_VOLUME" /dev/vg00/lv_data
```

### Step 7: Create Mount Point and Mount

```bash
# Create mount directory
mkdir /mnt/data
mkdir /mnt/backup

# Mount temporarily
mount /dev/vg00/lv_data /mnt/data

# Verify mount
df -h
```

### Step 8: Configure Permanent Mounting

Edit `/etc/fstab` for automatic mounting at boot:

```bash
# Edit fstab file
vim /etc/fstab

# Add these lines:
/dev/vg00/lv_data    /mnt/data    ext4    defaults    0    2
/dev/vg00/lv_backup  /mnt/backup  xfs     defaults    0    2
```

**Test fstab configuration:**
```bash
# Unmount
umount /mnt/data

# Test mount from fstab
mount -a

# Verify
df -h
```

## Common LVM Commands

### Physical Volume Commands
```bash
pvcreate    # Create PV
pvremove    # Remove PV
pvdisplay   # Display PV information
pvs         # Show PV summary
pvscan      # Scan for PVs
```

### Volume Group Commands
```bash
vgcreate    # Create VG
vgextend    # Add PV to VG
vgreduce    # Remove PV from VG
vgremove    # Remove VG
vgdisplay   # Display VG information
vgs         # Show VG summary
vgscan      # Scan for VGs
```

### Logical Volume Commands
```bash
lvcreate    # Create LV
lvextend    # Extend LV size
lvreduce    # Reduce LV size
lvremove    # Remove LV
lvdisplay   # Display LV information
lvs         # Show LV summary
lvscan      # Scan for LVs
```

## Practical Examples

### Example 1: Basic LVM Setup

```bash
# 1. Create Physical Volumes
pvcreate /dev/sdb /dev/sdc

# 2. Create Volume Group
vgcreate data_vg /dev/sdb /dev/sdc

# 3. Create Logical Volume (50GB)
lvcreate -L 50G -n web_data data_vg

# 4. Format with ext4
mkfs.ext4 /dev/data_vg/web_data

# 5. Create mount point and mount
mkdir /var/www/data
mount /dev/data_vg/web_data /var/www/data

# 6. Add to fstab
echo "/dev/data_vg/web_data /var/www/data ext4 defaults 0 2" >> /etc/fstab
```

### Example 2: Extending Logical Volume

```bash
# Check current size
df -h /var/www/data

# Extend LV by 20GB
lvextend -L +20G /dev/data_vg/web_data

# Resize filesystem (for ext4)
resize2fs /dev/data_vg/web_data

# For XFS filesystem, use:
# xfs_growfs /var/www/data

# Verify new size
df -h /var/www/data
```

### Example 3: Creating LVM Snapshot

```bash
# Create snapshot (10% of original size)
lvcreate -L 5G -s -n web_data_snapshot /dev/data_vg/web_data

# Mount snapshot
mkdir /mnt/snapshot
mount /dev/data_vg/web_data_snapshot /mnt/snapshot

# Remove snapshot when done
umount /mnt/snapshot
lvremove /dev/data_vg/web_data_snapshot
```

## Best Practices

1. **Naming Convention**: Use descriptive names for VGs and LVs
2. **Size Planning**: Don't allocate all VG space immediately
3. **Monitoring**: Regularly monitor space usage with `vgs`, `lvs`
4. **Backups**: Create snapshots before major changes
5. **Documentation**: Document your LVM layout and purpose
6. **Testing**: Test resize operations in non-production environments

## Troubleshooting

### Common Issues and Solutions

**PV not recognized:**
```bash
# Rescan devices
pvscan
partprobe
```

**Cannot remove PV:**
```bash
# Move data to other PVs first
pvmove /dev/sdb1
vgreduce vg00 /dev/sdb1
pvremove /dev/sdb1
```

**LV won't mount:**
```bash
# Check filesystem
fsck /dev/vg00/lv_data

# Check if LV is active
lvchange -ay /dev/vg00/lv_data
```

**VG not found after reboot:**
```bash
# Scan and activate
vgscan
vgchange -ay
```

### Useful Monitoring Commands

```bash
# Check overall LVM status
lvs -o +lv_size,data_percent

# Monitor space usage
vgs -o +vg_free_count,vg_extent_count

# Check PV usage
pvs -o +pv_used,pv_free
```

## Summary

LVM provides a flexible and powerful way to manage disk storage in Linux. The key steps are:

1. **Install** new hard drives
2. **Create** Physical Volumes (PV)
3. **Group** PVs into Volume Groups (VG)
4. **Create** Logical Volumes (LV) from VGs
5. **Format** LVs with filesystems
6. **Mount** and configure for permanent use

With LVM, you can easily resize, snapshot, and manage your storage without the limitations of traditional partitioning schemes.
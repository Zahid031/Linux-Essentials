# Linux File System Management & Storage for DevOps

Understanding and managing storage is a core DevOps responsibility. This guide covers the essential tools for inspecting, creating, and managing file systems and block devices in Linux.

### Table of Contents
1.  [Inspecting Block Devices](#1-inspecting-block-devices)
2.  [Partitioning & File System Creation](#2-partitioning--file-system-creation)
3.  [Mounting & Unmounting](#3-mounting--unmounting)
4.  [Logical Volume Management (LVM)](#4-logical-volume-management-lvm)

---

## 1. Inspecting Block Devices

Before you can manage storage, you need to know what you have.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `lsblk` | **L**i**s**t **Bl**oc**k** devices. Shows a tree view of disks, partitions, and their mount points. | `lsblk`: The standard, clean view. <br> `lsblk -f`: Shows **f**ile system type, UUID, and label. <br> `lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,VENDOR`: Customize the output columns. |
| `df` | **D**isk **F**ree. Reports file system disk space usage. | `df -h`: The go-to command. Shows usage in **h**uman-readable format (KB, MB, GB). <br> `df -i`: Show inode usage instead of block usage. |
| `du` | **D**isk **U**sage. Summarizes disk usage of files and directories. | `du -sh /var/log`: Show a **s**ummary of the total size of a directory in **h**uman-readable format. <br> `du -ah /path | sort -hr | head -n 10`: Find the top 10 largest files/directories in a path. |

---

## 2. Partitioning & File System Creation

These commands allow you to prepare a raw disk for use by the operating system.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `fdisk` | A powerful, classic tool for manipulating disk partition tables. | `sudo fdisk /dev/sdb`: Start an interactive session for the `sdb` disk. (Use `g` for GPT, `n` for new partition, `w` to write changes). |
| `parted` | A more modern, scriptable partitioning tool. | `sudo parted /dev/sdb mklabel gpt`: Create a new GPT partition table. <br> `sudo parted -a optimal /dev/sdb mkpart primary ext4 0% 100%`: Create a primary partition using the whole disk. |
| `mkfs` | **M**a**k**e a **f**ile **s**ystem on a device (partition). | `sudo mkfs.ext4 /dev/sdb1`: Format the first partition on `sdb` with the `ext4` file system. <br> `sudo mkfs.xfs /dev/sdb1`: Format with the `xfs` file system. |

> **Warning:** Partitioning and formatting are destructive operations. Always double-check the device name (`/dev/sdb`, `/dev/sdc`, etc.) before proceeding.

---

## 3. Mounting & Unmounting

Attaching a file system to a directory in the main file tree.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `mount` | Mount a file system. | `sudo mount /dev/sdb1 /mnt/data`: Mount the `sdb1` partition to the `/mnt/data` directory. |
| `umount` | Unmount a file system. | `sudo umount /mnt/data`: Unmount the device at the specified mount point. |
| `/etc/fstab` | A system configuration file that defines how partitions are mounted at boot time. | `UUID=<uuid_from_lsblk> /data ext4 defaults 0 2`: An example `fstab` entry to mount a device by its UUID. Using UUIDs is more robust than using device names like `/dev/sdb1`. |

---

## 4. Logical Volume Management (LVM)

LVM provides a more flexible way to manage disk space than traditional partitioning. It allows you to create, resize, and merge partitions without downtime.

### LVM Concepts
*   **Physical Volume (PV):** A physical disk or partition (`/dev/sda1`, `/dev/sdb`).
*   **Volume Group (VG):** A pool of storage made up of one or more PVs.
*   **Logical Volume (LV):** A "virtual" partition carved out of a VG, which is then formatted and mounted.

### Common LVM Commands

| Command | Purpose | Example |
| :--- | :--- | :--- |
| `pvcreate` | Initialize a disk or partition for use by LVM. | `sudo pvcreate /dev/sdb1` |
| `vgcreate` | Create a new Volume Group from one or more PVs. | `sudo vgcreate my_vg /dev/sdb1` |
| `lvcreate` | Create a new Logical Volume from a VG. | `sudo lvcreate -n my_lv -L 10G my_vg`: Create a 10GB LV named `my_lv`. |
| `lvextend` | Extend the size of an LV. | `sudo lvextend -L +5G /dev/my_vg/my_lv --resizefs`: Add 5GB to the LV and automatically resize the file system. |
| `pvs`, `vgs`, `lvs` | Display information about PVs, VGs, and LVs. | `sudo vgs` |

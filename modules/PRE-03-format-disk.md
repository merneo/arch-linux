# Module: Format Disk (Complete Wipe)

**Purpose:** Completely wipe and format disk before installation (for fresh installs)

**Prerequisites:**
- Booted from Arch Linux Live USB
- Disk identified
- **All important data backed up** (this will delete everything)

**Time:** 5-10 minutes

**ENVIRONMENT:** Live USB (root@archiso)

**Note:** This module is **OPTIONAL**. Only use if:
- You want to completely wipe the disk (remove all partitions)
- You're doing a fresh install (no existing data to preserve)
- You're not dual booting (single boot scenario)

**⚠️ WARNING:** If you're dual booting, **DO NOT** use this module. Use `06-disk-partitioning.md` instead to create partitions alongside Windows.

---

## ⚠️ WARNING: Data Loss

**This module will PERMANENTLY DELETE ALL DATA on the target disk.**

- **ALL partitions will be deleted**
- **ALL data will be lost**
- **This operation CANNOT be undone**
- **Make sure you have backups of important data**
- **Double-check disk names before proceeding** (use `lsblk` to verify)
- **Verify you're working with the correct disk** - wrong disk selection will destroy data

---

## When to Use This Module

**Use this module if:** For comprehensive guidance on disk partitioning, refer to the [ArchWiki on Partitioning](https://wiki.archlinux.org/title/Partitioning).
- ✅ You want a fresh install (no existing data)
- ✅ You're doing single boot (Arch Linux only)
- ✅ You want to completely wipe the disk

**DO NOT use this module if:**
- ❌ You're dual booting (Windows already installed)
- ❌ You want to preserve existing data
- ❌ You want to keep existing partitions

**For dual boot or preserving data:** Use [`06-disk-partitioning.md`](06-disk-partitioning.md) instead.

---

## Step 1: Identify Disk Device

```bash
# List all block devices. For more details on lsblk, refer to the [ArchWiki on lsblk](https://wiki.archlinux.org/title/Lsblk).
lsblk

# Identify target disk (usually the largest one, NOT the USB drive)
# USB drives are typically smaller (8-32 GB)
# Note device name: /dev/sda or /dev/nvme0n1
```

**Example output:**
```
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop0         7:0    0 686.6M  1 loop /run/archiso/airootfs
sda           8:0    0 500.1G  0 disk
├─sda1        8:1    0   512M  0 part
├─sda2        8:2    0 100.0G  0 part
└─sda3        8:3    0 399.6G  0 part
sdb           8:16   1  14.9G  0 disk    ← USB drive (smaller)
└─sdb1        8:17   1  14.9G  0 part
```

In this example, target disk is `/dev/sda` (500 GB, larger than USB).

---

## Step 2: Unmount All Partitions

```bash
# Unmount all partitions on target disk. For more details on unmounting filesystems, refer to the [ArchWiki on File systems#Mounting](https://wiki.archlinux.org/title/File_systems#Mounting).
# Replace /dev/sdX with your actual disk device

# For SATA/IDE disks:
umount /dev/sdX* 2>/dev/null

# For NVMe disks:
umount /dev/nvme0n1p* 2>/dev/null

# Verify nothing is mounted
lsblk
```

**Note:** If partitions are in use, you may need to use `umount -l` (lazy unmount) or reboot.

---

## Step 3: Wipe Disk (Delete All Partitions)

**⚠️ CRITICAL:** This will delete ALL partitions and data on the disk. For more information on securely wiping a disk, refer to the [ArchWiki on Securely wipe disk](https://wiki.archlinux.org/title/Securely_wipe_disk).

### Option A: Using wipefs (Recommended)

```bash
# Wipe all filesystem signatures from disk. For details on wipefs, consult 'man wipefs'.
# Replace /dev/sdX with your actual disk device

# For SATA/IDE disks:
wipefs -a /dev/sdX

# For NVMe disks:
wipefs -a /dev/nvme0n1

# Verify disk is wiped
lsblk
```

**Expected output:**
```
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda           8:0    0 500.1G  0 disk    ← No partitions, clean disk
```

### Option B: Using dd (Complete Wipe - Slower)

**⚠️ WARNING:** This method is slower but completely wipes the disk including partition table. For more details on 'dd', consult 'man dd' and the [ArchWiki on dd](https://wiki.archlinux.org/title/Dd).

```bash
# Wipe first 1 MB of disk (removes partition table)
# Replace /dev/sdX with your actual disk device

# For SATA/IDE disks:
dd if=/dev/zero of=/dev/sdX bs=1M count=1 status=progress

# For NVMe disks:
dd if=/dev/zero of=/dev/nvme0n1 bs=1M count=1 status=progress

# Sync to ensure write completes
sync
```

**Note:** This only wipes the partition table. For complete data wipe (secure erase), see Advanced section below.

---

## Step 4: Verify Disk is Wiped

```bash
# List block devices
lsblk

# Should show disk with no partitions
# Example:
# sda           8:0    0 500.1G  0 disk    ← Clean, no partitions
```

---

## Step 5: Create New Partition Table (Optional)

**If you want to create a new partition table immediately:** For more information on partition tables, refer to the [ArchWiki on Partitioning#Partition table](https://wiki.archlinux.org/title/Partitioning#Partition_table) and the [ArchWiki on parted](https://wiki.archlinux.org/title/Parted).

```bash
# Create new GPT partition table
# Replace /dev/sdX with your actual disk device

# For SATA/IDE disks:
parted /dev/sdX mklabel gpt

# For NVMe disks:
parted /dev/nvme0n1 mklabel gpt

# Verify
lsblk
```

**Note:** This step is optional. You can create partition table later in `06-disk-partitioning.md`.

---

## Advanced: Secure Erase (Complete Data Wipe)

**If you want to securely erase all data (for security or selling computer):** For more details, refer to the [ArchWiki on Securely wipe disk](https://wiki.archlinux.org/title/Securely_wipe_disk).

### For SATA/IDE Disks:

```bash
# Check if disk supports secure erase. For 'hdparm' usage, consult 'man hdparm'.
hdparm -I /dev/sdX | grep -i "supported\|frozen"

# If supported, unfreeze disk (if frozen)
hdparm -I /dev/sdX

# Secure erase (takes time, depends on disk size)
hdparm --security-erase NULL /dev/sdX
```

### For NVMe Disks:

```bash
# Check if disk supports secure erase. For 'nvme' usage, consult 'man nvme'.
nvme id-ctrl /dev/nvme0n1 | grep -i "format\|sanitize"

# Secure erase (takes time, depends on disk size)
nvme format /dev/nvme0n1 --ses=1
```

**Note:** Secure erase can take hours for large disks. Only use if you need complete data destruction.

---

## Troubleshooting

### Problem: Cannot unmount partitions
**Solution:**
1. Check if partitions are in use: `lsof /dev/sdX*`
2. Use lazy unmount: `umount -l /dev/sdX*`
3. Reboot if necessary

### Problem: wipefs fails
**Solution:**
1. Verify disk is not in use
2. Try `wipefs -a -f /dev/sdX` (force)
3. Use `dd` method instead

### Problem: Wrong disk selected
**Solution:**
1. **STOP immediately** if you realize you selected wrong disk
2. Data may already be lost, but stop to prevent further damage
3. Use `lsblk` to carefully identify correct disk
4. Double-check device name before running commands

---

**SUCCESS:** Disk wiped and ready for fresh installation

**Next:**
- `06-disk-partitioning.md` - Create new partitions for Arch Linux
- `07-luks-encryption.md` - Encrypt partitions (if needed)
- `08-btrfs-filesystem.md` - Create Btrfs filesystem

**Official Resources:**
- [ArchWiki: Partitioning](https://wiki.archlinux.org/title/Partitioning)
- [ArchWiki: Securely wipe disk](https://wiki.archlinux.org/title/Securely_wipe_disk)

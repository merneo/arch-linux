# Module: Disk Partitioning

**Purpose:** Create disk partitions for Arch Linux installation. Partitioning is a fundamental step in preparing a storage device for use by an operating system. For a comprehensive understanding of disk partitioning in Arch Linux, refer to the [ArchWiki on Partitioning](https://wiki.archlinux.org/title/Partitioning) and the utility [cfdisk](https://wiki.archlinux.org/title/Cfdisk).

**Prerequisites:**
- Booted from Arch Linux Live USB
- Disk identified

**Time:** 10-15 minutes

**Note:** This module is **OPTIONAL**. Only use if your disk is not already partitioned. If disk is already partitioned, skip to mounting modules (`btrfs-filesystem.md` or `mount-partitions.md`).

**ENVIRONMENT:** Live USB (root@archiso)

---

## ⚠️ WARNING: Data Loss

**This module will PERMANENTLY DELETE all data on the target disk.**

- Make sure you have backups of important data
- Double-check disk names before proceeding (use `lsblk` to verify)
- This operation CANNOT be undone
- Verify you're working with the correct disk - **wrong disk selection will destroy data**

---

## Step 1: Identify Disk Device

```bash
# List all block devices. For more details on lsblk, refer to the [ArchWiki on lsblk](https://wiki.archlinux.org/title/Lsblk).
lsblk

# Identify target disk (usually the largest one, NOT the USB drive)
# USB drives are typically smaller (8-32 GB)
# Note device name: /dev/sda or /dev/nvme0n1
```

---

## Step 2: Launch cfdisk

`cfdisk` is a curses-based disk partition manipulator, providing a user-friendly interface for creating and managing disk partitions. For details on `cfdisk`, refer to the [ArchWiki on cfdisk](https://wiki.archlinux.org/title/Cfdisk). When prompted, select `gpt` (GUID Partition Table), which is the standard for modern UEFI systems. See [ArchWiki on GUID Partition Table](https://wiki.archlinux.org/title/Partitioning#GUID_Partition_Table) for more information.

```bash
# Open cfdisk for your disk (replace /dev/sdX with your actual device)
cfdisk /dev/sdX

# For NVMe drives:
cfdisk /dev/nvme0n1

# Partition table type should be: gpt
# If asked "Select label type", choose: gpt
```

---

## Step 3: Choose Installation Scenario

**Before creating partitions, decide your installation scenario:**

- **Single Boot (Arch Linux only)** → Go to [Single Boot Section](#single-boot-arch-linux-only)
- **Dual Boot (Arch Linux + Windows)** → Go to [Dual Boot Section](#dual-boot-arch-linux--windows)

---

## Single Boot (Arch Linux Only)

**Use this if:** You want to install Arch Linux on the entire disk (no Windows).

### Create EFI System Partition

For UEFI systems, an [EFI system partition (ESP)](https://wiki.archlinux.org/title/EFI_system_partition) is required. It stores boot loaders and must be formatted as FAT32. The `EF00` type code identifies it as an ESP.

1. Navigate to **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `512M` (minimum 512 MB for EFI) and press Enter
4. Navigate to the newly created partition
5. Press Enter on **[ Type ]** menu option
6. Scroll down to find **EFI System** (type code EF00)
7. Press Enter to select EFI System

### Create Linux Root Partition

This partition will house your root filesystem. For information on various [Linux filesystems](https://wiki.archlinux.org/title/File_systems), refer to the ArchWiki.

1. Navigate to remaining **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `90%` (or leave remaining space for Arch Linux) and press Enter
4. Partition type will default to **Linux filesystem** (correct, do not change)

### Create Swap Partition (Optional)

A [swap partition](https://wiki.archlinux.org/title/Swap) provides virtual memory when physical RAM is exhausted and can be used for hibernation.

1. Navigate to remaining **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `10%` (or `8G` minimum) and press Enter
4. Navigate to the newly created partition
5. Press Enter on **[ Type ]** menu option
6. Scroll down to find **Linux swap** (type code 19 or 82)
7. Press Enter to select Linux swap

**Expected Layout:**
- EFI System Partition (512 MB)
- Linux Root Partition (90% of disk)
- Linux Swap (10% of disk, optional)

---

## Dual Boot (Arch Linux + Windows)

**Use this if:** You want to keep Windows and install Arch Linux alongside it. For a complete guide on dual-booting with Windows, refer to the [ArchWiki on Dual boot with Windows](https://wiki.archlinux.org/title/Dual_boot_with_Windows).

**⚠️ IMPORTANT: Before proceeding:**
- Windows Fast Startup **MUST BE DISABLED** (prevents filesystem corruption). See [ArchWiki: Dual boot with Windows#Disable Fast Startup](https://wiki.archlinux.org/title/Dual_boot_with_Windows#Disable_Fast_Startup).
- BitLocker encryption in Windows **MUST BE DISABLED**. See [ArchWiki: Dual boot with Windows#Disable BitLocker](https://wiki.archlinux.org/title/Dual_boot_with_Windows#Disable_BitLocker).
- Back up important data before resizing partitions.

### Option A: Use Free Space (Windows Already Installed)

**If Windows is already installed and you have free space:**

1. Check existing partitions with `lsblk`
2. Identify free space (unallocated space on disk)
3. Use free space for Arch Linux partitions

**Create Linux Root Partition:**
1. Navigate to **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Use available free space (e.g., `70G` or `50%` of free space)
4. Partition type will default to **Linux filesystem** (correct, do not change)

**Create Swap Partition (Optional):**
1. Navigate to remaining **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `8G` (minimum) and press Enter
4. Navigate to the newly created partition
5. Press Enter on **[ Type ]** menu option
6. Scroll down to find **Linux swap** (type code 19 or 82)
7. Press Enter to select Linux swap

**Note:** EFI System Partition should already exist (created by Windows). Verify with `lsblk -f` - it should show FAT32 [EFI system partition](https://wiki.archlinux.org/title/EFI_system_partition).

### Option B: Resize Windows Partition (Advanced)

**If you need to shrink Windows partition to make space:**

**⚠️ WARNING:** Resizing Windows partitions can cause data loss. Always back up first.

1. Use Windows Disk Management or `gparted` to shrink Windows partition
2. Leave free space for Arch Linux
3. Then follow "Option A" above to create Arch Linux partitions

**Expected Layout:**
- EFI System Partition (512 MB, shared with Windows)
- Windows partition (NTFS)
- Linux Root Partition (Arch Linux)
- Linux Swap (optional)

---

**Continue to Step 4** after creating partitions for your chosen scenario.

---

## Step 4: Write Changes

1. Navigate to **[ Write ]** menu option
2. Press Enter
3. **Confirmation prompt**: `Are you sure you want to write the partition table to disk? (yes or no):`
4. Type exactly: `yes` (lowercase, no quotes)
5. Press Enter
6. Navigate to **[ Quit ]** menu option
7. Press Enter to exit cfdisk

---

## Step 5: Verify Partitions

After writing changes to the disk, it's essential to ensure the kernel re-reads the partition table and that the new partitions are visible.

```bash
# Force kernel to re-read partition table. For more on 'partprobe', see its [man page](https://man.archlinux.org/man/partprobe.8).
partprobe /dev/sdX

# List partitions again. Verify with [ArchWiki: lsblk](https://wiki.archlinux.org/title/Lsblk).
lsblk -f

# Should show:
# sdX
# ├─sdX1  vfat   FAT32  (EFI System)
# ├─sdX2          (Linux root - empty)
# └─sdX3          (Linux swap - empty, optional)
```

---

**SUCCESS:** Disk partitioned successfully

**Next:** Choose your filesystem option:
- **Btrfs + LUKS (encrypted):** `luks-encryption.md` → `btrfs-filesystem.md`
- **Btrfs only (no encryption):** `btrfs-filesystem.md` (skip LUKS)
- **Other filesystems:** `mount-partitions.md`

**See also:** `INSTALLATION-SCENARIOS.md` for detailed scenario guidance

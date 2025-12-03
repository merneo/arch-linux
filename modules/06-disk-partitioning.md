# Module: Disk Partitioning

**Purpose:** Create disk partitions for Arch Linux installation

**Prerequisites:**
- Booted from Arch Linux Live USB
- Disk identified

**Time:** 10-15 minutes

**Note:** This module is **OPTIONAL**. Only use if your disk is not already partitioned. If disk is already partitioned, skip to mounting modules (`08-btrfs-filesystem.md` or `09-mount-partitions.md`).

---

## Step 1: Identify Disk Device

```bash
# List all block devices
lsblk

# Identify target disk (usually the largest one, NOT the USB drive)
# USB drives are typically smaller (8-32 GB)
# Note device name: /dev/sda or /dev/nvme0n1
```

---

## Step 2: Launch cfdisk

```bash
# Open cfdisk for your disk (replace /dev/sdX with your actual device)
cfdisk /dev/sdX

# For NVMe drives:
cfdisk /dev/nvme0n1

# Partition table type should be: gpt
# If asked "Select label type", choose: gpt
```

---

## Step 3: Create Partitions

**Create EFI System Partition:**
1. Navigate to **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `512M` (minimum 512 MB for EFI) and press Enter
4. Navigate to the newly created partition
5. Press Enter on **[ Type ]** menu option
6. Scroll down to find **EFI System** (type code EF00)
7. Press Enter to select EFI System

**Create Linux Root Partition:**
1. Navigate to remaining **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `70%` (or specific size like `350G`) and press Enter
4. Partition type will default to **Linux filesystem** (correct, do not change)

**Create Swap Partition (Optional):**
1. Navigate to remaining **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `5%` (or `8G` minimum) and press Enter
4. Navigate to the newly created partition
5. Press Enter on **[ Type ]** menu option
6. Scroll down to find **Linux swap** (type code 19 or 82)
7. Press Enter to select Linux swap

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

```bash
# Force kernel to re-read partition table
partprobe /dev/sdX

# List partitions again
lsblk -f

# Should show:
# sdX
# ├─sdX1  vfat   FAT32  (EFI System)
# ├─sdX2          (Linux root - empty)
# └─sdX3          (Linux swap - empty, optional)
```

---

**SUCCESS:** Disk partitioned successfully

**Next:** 
- `07-luks-encryption.md` - Encrypt partitions (if needed)
- `08-btrfs-filesystem.md` - Create Btrfs filesystem
- `09-mount-partitions.md` - Mount partitions before installation

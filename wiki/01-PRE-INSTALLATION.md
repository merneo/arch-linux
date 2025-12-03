# Pre-Installation Steps

**Purpose:** Prepare disk and filesystem before installing the system

**When to use:** Only if your disk is not already partitioned and mounted at `/mnt`

---

## Menu: Pre-Installation Steps

**⚠️ IMPORTANT:** Before proceeding, complete [Phase 00: Preparation](../phases/00-PREPARATION.md):

1. **[Create Arch Linux USB](../steps/PRE-01-create-arch-usb.md)** - Create bootable USB drive (required)
2. **[Install Windows](../steps/PRE-02-install-windows.md)** - Only if dual booting (optional)
3. **[Format Disk](../steps/PRE-03-format-disk.md)** - Only if you want to completely wipe disk (optional)

**Then choose your installation scenario:** See [Installation Scenarios](../INSTALLATION-SCENARIOS.md) for detailed guidance.

**Recommended:** Follow [Phase 01: Disk Setup](../phases/01-DISK_SETUP.md) for organized disk configuration.

**Or choose individual steps:**

- **[Disk Partitioning](#disk-partitioning)** - Create disk partitions (single boot or dual boot)
- **[LUKS Encryption](#luks-encryption)** - Encrypt partitions (optional, for Btrfs + LUKS scenario)
- **[Btrfs Filesystem](#btrfs-filesystem)** - Create Btrfs with subvolumes (with or without LUKS)
- **[Mount Partitions](#mount-partitions)** - Mount partitions (non-Btrfs filesystems)

---

## Disk Partitioning

**Purpose:** Create disk partitions for Arch Linux installation

**Prerequisites:**
- Booted from Arch Linux Live USB
- Disk identified

**Time:** 10-15 minutes

**Note:** This step is **OPTIONAL**. Only use if your disk is not already partitioned.

### Step 1: Identify Disk Device

```bash
# List all block devices
lsblk

# Identify target disk (usually the largest one, NOT the USB drive)
# USB drives are typically smaller (8-32 GB)
# Note device name: /dev/sda or /dev/nvme0n1
```

### Step 2: Launch cfdisk

```bash
# Open cfdisk for your disk (replace /dev/sdX with your actual device)
cfdisk /dev/sdX

# For NVMe drives:
cfdisk /dev/nvme0n1

# Partition table type should be: gpt
# If asked "Select label type", choose: gpt
```

### Step 3: Create Partitions

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

### Step 4: Write Changes

1. Navigate to **[ Write ]** menu option
2. Press Enter
3. **Confirmation prompt**: `Are you sure you want to write the partition table to disk? (yes or no):`
4. Type exactly: `yes` (lowercase, no quotes)
5. Press Enter
6. Navigate to **[ Quit ]** menu option
7. Press Enter to exit cfdisk

### Step 5: Verify Partitions

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

**SUCCESS:** Disk partitioned successfully

**Next:** 
- [LUKS Encryption](#luks-encryption) - Encrypt partitions (if needed)
- [Btrfs Filesystem](#btrfs-filesystem) - Create Btrfs filesystem
- [Mount Partitions](#mount-partitions) - Mount partitions (non-Btrfs)
- [Core Installation](02-CORE-INSTALLATION.md) - If partitions are already mounted

---

## LUKS Encryption

**Purpose:** Encrypt disk partitions with LUKS2

**Prerequisites:**
- Disk partitioned (see [Disk Partitioning](#disk-partitioning) above)
- Root partition identified (e.g., `/dev/sdX2`)

**Time:** 10-15 minutes

### Step 1: Encrypt Root Partition

```bash
# Replace /dev/sdX2 with your root partition
cryptsetup luksFormat --type luks2 \
  --cipher aes-xts-plain64 \
  --key-size 512 \
  --pbkdf argon2id \
  --iter-time 5000 \
  /dev/sdX2
```

**Prompt 1: Confirmation**
```
WARNING!
========
This will overwrite data on /dev/sdX2 irrevocably.

Are you sure? (Type 'yes' in capital letters):
```

**Type exactly:** `YES` (uppercase) and press Enter

**Prompt 2: Passphrase**
```
Enter passphrase for /dev/sdX2:
```

**Enter a STRONG passphrase** (minimum 20 characters recommended)

**Prompt 3: Verify passphrase**
```
Verify passphrase:
```

**Re-type the EXACT SAME passphrase** and press Enter

**Wait for completion** (30-60 seconds)

### Step 2: Open Encrypted Root

```bash
# Unlock LUKS container and map to /dev/mapper/cryptroot
cryptsetup open /dev/sdX2 cryptroot

# Enter the passphrase you just created
```

**Verify:**
```bash
ls -la /dev/mapper/

# Should show:
# cryptroot -> ../dm-0
```

### Step 3: Encrypt Swap Partition (Optional)

```bash
# Replace /dev/sdX3 with your swap partition
cryptsetup luksFormat --type luks2 \
  --cipher aes-xts-plain64 \
  --key-size 512 \
  --pbkdf argon2id \
  --iter-time 5000 \
  /dev/sdX3

# Type YES, enter passphrase (can be same as root)

# Open encrypted swap
cryptsetup open /dev/sdX3 cryptswap
```

### Step 4: Verify Encryption

```bash
# Display LUKS header information
cryptsetup luksDump /dev/sdX2 | head -20

# Should show:
# LUKS header information for /dev/sdX2
# Version:        2
# Cipher name:    aes
# Cipher mode:    xts-plain64
# Hash spec:      sha512
# Key Slot 0: ENABLED
```

**SUCCESS:** Partitions encrypted with LUKS2

**Next:**
- [Btrfs Filesystem](#btrfs-filesystem) - Create Btrfs filesystem on encrypted partition
- [Mount Partitions](#mount-partitions) - Mount partitions (non-Btrfs)
- [Core Installation](02-CORE-INSTALLATION.md) - If partitions are already mounted

---

## Btrfs Filesystem

**Purpose:** Create Btrfs filesystem with subvolumes

**Prerequisites:**
- Root partition ready (encrypted or unencrypted)
  - If encrypted: LUKS container opened (see [LUKS Encryption](#luks-encryption) above)
  - If unencrypted: Partition identified (e.g., `/dev/sdX2`)

**Time:** 5-10 minutes

### Step 1: Format with Btrfs

**For encrypted partition:**
```bash
mkfs.btrfs -L "Arch Linux" /dev/mapper/cryptroot
```

**For unencrypted partition:**
```bash
mkfs.btrfs -L "Arch Linux" /dev/sdX2
```

### Step 2: Mount Btrfs Temporarily

**For encrypted:**
```bash
mount /dev/mapper/cryptroot /mnt
```

**For unencrypted:**
```bash
mount /dev/sdX2 /mnt
```

### Step 3: Create Btrfs Subvolumes

```bash
# Create @ subvolume (root filesystem)
btrfs subvolume create /mnt/@

# Create @home subvolume (user home directories)
btrfs subvolume create /mnt/@home

# Create @log subvolume (system logs)
btrfs subvolume create /mnt/@log

# Create @cache subvolume (package cache)
btrfs subvolume create /mnt/@cache

# Create @snapshots subvolume (snapshot storage)
btrfs subvolume create /mnt/@snapshots
```

**Verify subvolumes:**
```bash
btrfs subvolume list /mnt

# Should show:
# ID 256 gen X top level 5 path @
# ID 257 gen X top level 5 path @home
# ID 258 gen X top level 5 path @log
# ID 259 gen X top level 5 path @cache
# ID 260 gen X top level 5 path @snapshots
```

### Step 4: Unmount and Remount with Subvolumes

```bash
# Unmount root
umount /mnt

# Mount @ subvolume as root with compression
# For encrypted:
mount -o subvol=@,compress=zstd,noatime /dev/mapper/cryptroot /mnt

# For unencrypted:
mount -o subvol=@,compress=zstd,noatime /dev/sdX2 /mnt
```

### Step 5: Create Mount Point Directories

```bash
mkdir -p /mnt/{home,var/log,var/cache,boot,.snapshots}
```

### Step 6: Mount All Btrfs Subvolumes

**For encrypted:**
```bash
mount -o subvol=@home,compress=zstd,noatime /dev/mapper/cryptroot /mnt/home
mount -o subvol=@log,compress=zstd,noatime /dev/mapper/cryptroot /mnt/var/log
mount -o subvol=@cache,compress=zstd,noatime /dev/mapper/cryptroot /mnt/var/cache
mount -o subvol=@snapshots,compress=zstd,noatime /dev/mapper/cryptroot /mnt/.snapshots
```

**For unencrypted:**
```bash
mount -o subvol=@home,compress=zstd,noatime /dev/sdX2 /mnt/home
mount -o subvol=@log,compress=zstd,noatime /dev/sdX2 /mnt/var/log
mount -o subvol=@cache,compress=zstd,noatime /dev/sdX2 /mnt/var/cache
mount -o subvol=@snapshots,compress=zstd,noatime /dev/sdX2 /mnt/.snapshots
```

### Step 7: Mount EFI Boot Partition

```bash
# Mount EFI partition (replace /dev/sdX1 with your EFI partition)
mount /dev/sdX1 /mnt/boot
```

### Step 8: Format and Enable Swap (if created)

**For encrypted swap:**
```bash
mkswap /dev/mapper/cryptswap
swapon /dev/mapper/cryptswap
```

**For unencrypted swap:**
```bash
mkswap /dev/sdX3
swapon /dev/sdX3
```

### Step 9: Verify All Mounts

```bash
lsblk -f

# Should show all mounted filesystems
```

**SUCCESS:** Btrfs filesystem with subvolumes created and mounted

**Next:**
- [Core Installation](02-CORE-INSTALLATION.md) - Install base system

---

## Mount Partitions

**Purpose:** Mount partitions for installation (if not using Btrfs)

**Prerequisites:**
- Disk partitioned (see [Disk Partitioning](#disk-partitioning) above)
- Filesystem created on root partition (ext4, xfs, etc.)

**Time:** 2-3 minutes

### Step 1: Create Filesystem on Root Partition

**For ext4:**
```bash
mkfs.ext4 -L "Arch Linux" /dev/sdX2
```

**For xfs:**
```bash
mkfs.xfs -L "Arch Linux" /dev/sdX2
```

### Step 2: Mount Root Partition

```bash
mount /dev/sdX2 /mnt
```

### Step 3: Mount EFI Boot Partition

```bash
# Format EFI partition (if not already formatted)
mkfs.fat -F32 /dev/sdX1

# Mount EFI partition
mount /dev/sdX1 /mnt/boot
```

### Step 4: Enable Swap (if created)

```bash
mkswap /dev/sdX3
swapon /dev/sdX3
```

### Step 5: Verify Mounts

```bash
lsblk -f

# Should show:
# sdX
# ├─sdX1  vfat   FAT32  /mnt/boot
# ├─sdX2  ext4   Arch Linux  /mnt
# └─sdX3  swap   [SWAP]
```

**SUCCESS:** Partitions mounted and ready for installation

**Next:**
- [Core Installation](02-CORE-INSTALLATION.md) - Install base system

---

**Back to:** [Home](00-HOME.md) | **Next:** [Core Installation](02-CORE-INSTALLATION.md)

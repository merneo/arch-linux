# Pre-Installation Steps

**Purpose:** Prepare disk and filesystem before installing the system. This phase ensures your storage is correctly configured and secured according to your needs. For a comprehensive overview of Arch Linux pre-installation, refer to the [ArchWiki Installation Guide#Pre-installation](https://wiki.archlinux.org/title/Installation_guide#Pre-installation).

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

**Purpose:** Create disk partitions for Arch Linux installation. For a comprehensive understanding of disk partitioning in Arch Linux, refer to the [ArchWiki on Partitioning](https://wiki.archlinux.org/title/Partitioning) and the utility [cfdisk](https://wiki.archlinux.org/title/Cfdisk).

**Prerequisites:**
- Booted from Arch Linux Live USB
- Disk identified

**Time:** 10-15 minutes

**Note:** This step is **OPTIONAL**. Only use if your disk is not already partitioned.

### Step 1: Identify Disk Device

```bash
# List all block devices. For more details on lsblk, refer to the [ArchWiki on lsblk](https://wiki.archlinux.org/title/Lsblk).
lsblk

# Identify target disk (usually the largest one, NOT the USB drive)
# USB drives are typically smaller (8-32 GB)
# Note device name: /dev/sda or /dev/nvme0n1
```

### Step 2: Launch cfdisk

`cfdisk` is a curses-based disk partition manipulator, providing a user-friendly interface for creating and managing disk partitions. When prompted, select `gpt` (GUID Partition Table), which is the standard for modern UEFI systems. See [ArchWiki on GUID Partition Table](https://wiki.archlinux.org/title/Partitioning#GUID_Partition_Table) for more information.

```bash
# Open cfdisk for your disk (replace /dev/sdX with your actual device)
cfdisk /dev/sdX

# For NVMe drives:
cfdisk /dev/nvme0n1

# Partition table type should be: gpt
# If asked "Select label type", choose: gpt
```

### Step 3: Create Partitions

**Create EFI System Partition:** For UEFI systems, an [EFI system partition (ESP)](https://wiki.archlinux.org/title/EFI_system_partition) is required. It stores boot loaders and must be formatted as FAT32. The `EF00` type code identifies it as an ESP.
1. Navigate to **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `512M` (minimum 512 MB for EFI) and press Enter
4. Navigate to the newly created partition
5. Press Enter on **[ Type ]** menu option
6. Scroll down to find **EFI System** (type code EF00)
7. Press Enter to select EFI System

**Create Linux Root Partition:** This partition will house your root filesystem. For information on various [Linux filesystems](https://wiki.archlinux.org/title/File_systems), refer to the ArchWiki.
1. Navigate to remaining **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `70%` (or specific size like `350G`) and press Enter
4. Partition type will default to **Linux filesystem** (correct, do not change)

**Create Swap Partition (Optional):** A [swap partition](https://wiki.archlinux.org/title/Swap) provides virtual memory when physical RAM is exhausted and can be used for hibernation.
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

After writing changes to the disk, it's essential to ensure the kernel re-reads the partition table (`partprobe`) and that the new partitions are visible (`lsblk`). For more on `partprobe`, see its [man page](https://man.archlinux.org/man/partprobe.8). For `lsblk`, refer to the [ArchWiki on lsblk](https://wiki.archlinux.org/title/Lsblk).

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

**Purpose:** Encrypt disk partitions with LUKS2. LUKS (Linux Unified Key Setup) is the standard for Linux hard disk encryption, offering a robust and secure way to protect data at rest. For a comprehensive guide, refer to the [ArchWiki on dm-crypt/Encrypting an entire system](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system).

**Prerequisites:**
- Disk partitioned (see [Disk Partitioning](#disk-partitioning) above)
- Root partition identified (e.g., `/dev/sdX2`)

**Time:** 10-15 minutes

### Step 1: Encrypt Root Partition

The `cryptsetup luksFormat` command initializes a LUKS encrypted volume. The parameters specify the encryption algorithm and key derivation function. For more details on `cryptsetup` and its options, refer to the [ArchWiki on dm-crypt/Drive encryption](https://wiki.archlinux.org/title/Dm-crypt/Drive_encryption#Encryption_options) or consult `man cryptsetup`.

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

After encrypting a partition with `luksFormat`, it must be "opened" to make the underlying unencrypted device available. The `cryptsetup open` command decrypts the volume and maps it to a device in `/dev/mapper/`. For more details, refer to the [ArchWiki on dm-crypt/Device encryption](https://wiki.archlinux.org/title/Dm-crypt/Device_encryption#Using_dm-crypt).

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

Encrypting the swap partition is a recommended security measure to prevent sensitive data from being written to unencrypted storage. For best practices regarding swap encryption, refer to the [ArchWiki on dm-crypt/Swap encryption](https://wiki.archlinux.org/title/Dm-crypt/Swap_encryption).

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

The `cryptsetup luksDump` command displays detailed information about a LUKS-encrypted volume, including its version, cipher, hash, and key slots. This is useful for verifying the encryption parameters. For more details, consult `man cryptsetup` or the [ArchWiki on dm-crypt/Drive encryption](https://wiki.archlinux.org/title/Dm-crypt/Drive_encryption).

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

**Purpose:** Create Btrfs filesystem with subvolumes. Btrfs (B-tree file system) is a modern copy-on-write (CoW) filesystem for Linux aimed at implementing advanced features while focusing on fault tolerance, repair, and easy administration. Its key advantages include snapshots, subvolumes, and integrity checks. For comprehensive information, refer to the [ArchWiki on Btrfs](https://wiki.archlinux.org/title/Btrfs).

**Prerequisites:**
- Root partition ready (encrypted or unencrypted)
  - If encrypted: LUKS container opened (see [LUKS Encryption](#luks-encryption) above)
  - If unencrypted: Partition identified (e.g., `/dev/sdX2`)

**Time:** 5-10 minutes

### Step 1: Format with Btrfs

The `mkfs.btrfs` command is used to create a Btrfs filesystem on a partition. The `-L` option sets a label for the filesystem, which can be useful for identification. For further details, refer to `man mkfs.btrfs` and the [ArchWiki on Btrfs#Formatting](https://wiki.archlinux.org/title/Btrfs#Formatting).

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

[Btrfs subvolumes](https://wiki.archlinux.org/title/Btrfs#Subvolumes) are independently mountable POSIX file trees that share a common disk space with other subvolumes. They are not like traditional logical volumes; rather, they are flexible units within the Btrfs filesystem that can be snapshotted and managed individually. This setup is optimized for system snapshots and data separation.

-   `@`: This subvolume will serve as the root filesystem (`/`).
-   `@home`: This subvolume is dedicated to user home directories (`/home`), allowing for easier backups or reinstallation of the root without affecting user data.
-   `@log`: For system logs (`/var/log`). Isolating logs can prevent them from filling up the root filesystem.
-   `@cache`: For package caches (`/var/cache`).
-   `@snapshots`: Dedicated storage for filesystem snapshots.

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

After creating subvolumes, the root filesystem needs to be unmounted and then re-mounted with the correct `subvol` option. Mount options like `compress=zstd` enable transparent compression (beneficial for performance and disk space) and `noatime` disables updating access times (improving SSD longevity). For a detailed list of mount options, see [ArchWiki: Btrfs#Mount options](https://wiki.archlinux.org/title/Btrfs#Mount_options).

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

The [EFI system partition (ESP)](https://wiki.archlinux.org/title/EFI_system_partition) is where the boot loader (like GRUB) will be installed. It must be mounted at `/mnt/boot`.

```bash
# Mount EFI partition (replace /dev/sdX1 with your EFI partition)
mount /dev/sdX1 /mnt/boot
```

### Step 8: Format and Enable Swap (if created)

The [swap partition](https://wiki.archlinux.org/title/Swap) provides virtual memory. The `mkswap` command initializes a Linux swap area on a device, and `swapon` enables devices for paging and swapping.

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

The `lsblk -f` command lists block devices, their filesystems, labels, UUIDs, and mountpoints. This is essential for verifying that all partitions, including subvolumes and the EFI partition, are correctly mounted. Refer to the [ArchWiki on lsblk](https://wiki.archlinux.org/title/Lsblk) for more details.

```bash
lsblk -f

# Should show all mounted filesystems
```

**SUCCESS:** Btrfs filesystem with subvolumes created and mounted

**Next:**
- [Core Installation](02-CORE-INSTALLATION.md) - Install base system

---

## Mount Partitions

**Purpose:** Mount partitions for installation (if not using Btrfs). For a deeper understanding of filesystem mounting, refer to the [ArchWiki on File systems#Mounting](https://wiki.archlinux.org/title/File_systems#Mounting).

**Prerequisites:**
- Disk partitioned (see [Disk Partitioning](#disk-partitioning) above)
- Filesystem created on root partition (ext4, xfs, etc.)

**Time:** 2-3 minutes

### Step 1: Create Filesystem on Root Partition

This step formats the designated root partition with a chosen filesystem. `ext4` is a journaling filesystem and is a common default for Linux, while `xfs` is known for its performance with large files and directories. For more details on these filesystems, refer to the [ArchWiki on Ext4](https://wiki.archlinux.org/title/Ext4) and [ArchWiki on XFS](https://wiki.archlinux.org/title/XFS).

**For ext4:**
```bash
mkfs.ext4 -L "Arch Linux" /dev/sdX2
```

**For xfs:**
```bash
mkfs.xfs -L "Arch Linux" /dev/sdX2
```

### Step 2: Mount Root Partition

The `mount` command attaches the filesystem on the specified device (your root partition) to the filesystem tree at the specified mount point (`/mnt`). For detailed information on the `mount` command and its options, refer to the [ArchWiki on Mount](https://wiki.archlinux.org/title/Mount).

```bash
mount /dev/sdX2 /mnt
```

### Step 3: Mount EFI Boot Partition

The [EFI system partition (ESP)](https://wiki.archlinux.org/title/EFI_system_partition) is a mandatory partition for UEFI systems, containing boot loaders and applications. It must be formatted as FAT32. The `mkfs.fat` command creates a FAT filesystem.

```bash
# Format EFI partition (if not already formatted)
mkfs.fat -F32 /dev/sdX1

# Mount EFI partition
mount /dev/sdX1 /mnt/boot
```

### Step 4: Enable Swap (if created)

The [swap partition](https://wiki.archlinux.org/title/Swap) provides virtual memory. The `mkswap` command initializes a Linux swap area on a device, and `swapon` enables devices for paging and swapping.

```bash
mkswap /dev/sdX3
swapon /dev/sdX3
```

### Step 5: Verify Mounts

The `lsblk -f` command lists block devices, their filesystems, labels, UUIDs, and mountpoints. This is essential for verifying that all partitions are correctly mounted. Refer to the [ArchWiki on lsblk](https://wiki.archlinux.org/title/Lsblk) for more details.

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

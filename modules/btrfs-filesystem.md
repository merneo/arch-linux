# Module: Btrfs Filesystem Creation

**Purpose:** Create Btrfs filesystem with subvolumes. Btrfs (B-tree file system) is a modern copy-on-write (CoW) filesystem for Linux aimed at implementing advanced features while focusing on fault tolerance, repair, and easy administration. Its key advantages include snapshots, subvolumes, and integrity checks. For comprehensive information, refer to the [ArchWiki on Btrfs](https://wiki.archlinux.org/title/Btrfs).

**Prerequisites:**
- Root partition ready (encrypted or unencrypted)
  - If encrypted: LUKS container opened (module `luks-encryption.md`)
  - If unencrypted: Partition identified (e.g., `<ROOT_PARTITION>`)

**Time:** 5-10 minutes

**ENVIRONMENT:** Live USB (root@archiso)

---

## ⚠️ WARNING: Data Loss

**This module will PERMANENTLY DELETE all data on the target partition.**

- Make sure you have backups of important data
- Double-check partition names before proceeding
- This operation CANNOT be undone

---

## Placeholders Used in This Module

- `<ROOT_PARTITION>`: Your root partition device (e.g., `/dev/sda2`, `/dev/nvme0n1p2`)
- `<EFI_PARTITION>`: Your EFI boot partition device (e.g., `/dev/sda1`, `/dev/nvme0n1p1`)

---

## Step 1: Format with Btrfs

The `mkfs.btrfs` command is used to create a Btrfs filesystem on a partition. The `-L` option sets a label for the filesystem, which can be useful for identification. For further details, refer to `man mkfs.btrfs` and the [ArchWiki on Btrfs#Formatting](https://wiki.archlinux.org/title/Btrfs#Formatting).

**For encrypted partition:**
```bash
mkfs.btrfs -L "Arch Linux" /dev/mapper/cryptroot
```

**For unencrypted partition:**
```bash
mkfs.btrfs -L "Arch Linux" /dev/sdX2
```

---

## Step 2: Mount Btrfs Temporarily

**For encrypted:**
```bash
mount /dev/mapper/cryptroot /mnt
```

**For unencrypted:**
```bash
mount /dev/sdX2 /mnt
```

---

## Step 3: Create Btrfs Subvolumes

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

---

## Step 4: Unmount and Remount with Subvolumes

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

---

## Step 5: Create Mount Point Directories

```bash
mkdir -p /mnt/{home,var/log,var/cache,boot,.snapshots}
```

---

## Step 6: Mount All Btrfs Subvolumes

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

---

## Step 7: Mount EFI Boot Partition

The [EFI system partition (ESP)](https://wiki.archlinux.org/title/EFI_system_partition) is where the boot loader (like GRUB) will be installed. It must be mounted at `/mnt/boot`.

```bash
# Mount EFI partition (replace /dev/sdX1 with your EFI partition)
mount /dev/sdX1 /mnt/boot
```

---

## Step 8: Format and Enable Swap (if created)

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

---

## Step 9: Verify All Mounts

The `lsblk -f` command lists block devices, their filesystems, labels, UUIDs, and mountpoints. This is essential for verifying that all partitions, including subvolumes and the EFI partition, are correctly mounted. Refer to the [ArchWiki on lsblk](https://wiki.archlinux.org/title/Lsblk) for more details.

```bash
lsblk -f

# Should show all mounted filesystems
```

---

**SUCCESS:** Btrfs filesystem with subvolumes created and mounted

---

## Troubleshooting

For more extensive troubleshooting on Btrfs, refer to the [ArchWiki on Btrfs#Troubleshooting](https://wiki.archlinux.org/title/Btrfs#Troubleshooting).

### Problem: Cannot create Btrfs filesystem
**Solution:**
1. Verify partition exists: `lsblk` or `fdisk -l`
2. Check if partition is in use: `mount | grep <PARTITION>`
3. Unmount if needed: `umount <PARTITION>`
4. Install Btrfs tools: `pacman -S btrfs-progs`

### Problem: Subvolumes not mounting correctly
**Solution:**
1. Verify subvolumes exist: `btrfs subvolume list /mnt`
2. Check mount options in fstab: `cat /etc/fstab`
3. Ensure subvol= option is correct: `subvol=@` for root, `subvol=@home` for home
4. Remount with correct options: `mount -o remount,subvol=@ /mnt`

### Problem: Btrfs filesystem errors
**Solution:**
1. Check filesystem: `btrfs check /dev/sdXY`
2. If errors found, use read-only check: `btrfs check --readonly /dev/sdXY`
3. For serious issues, refer to [ArchWiki: Btrfs#Troubleshooting](https://wiki.archlinux.org/title/Btrfs#Troubleshooting)

---

**Next:**
- `mount-partitions.md` - Verify mount setup (if not using Btrfs)
- `core-installation.md` - Install base system

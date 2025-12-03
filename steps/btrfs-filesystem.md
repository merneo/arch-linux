# Step: Btrfs Filesystem Creation

**Purpose:** Create Btrfs filesystem with subvolumes. For comprehensive information, refer to the [ArchWiki on Btrfs](https://wiki.archlinux.org/title/Btrfs).

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Root partition ready (encrypted or unencrypted)

## Commands

```bash
# Format with Btrfs. See [ArchWiki: Btrfs#Formatting](https://wiki.archlinux.org/title/Btrfs#Formatting).
# For encrypted:
mkfs.btrfs -L "Arch Linux" /dev/mapper/cryptroot

# For unencrypted:
mkfs.btrfs -L "Arch Linux" /dev/sdX2

# Mount temporarily
# For encrypted:
mount /dev/mapper/cryptroot /mnt

# For unencrypted:
mount /dev/sdX2 /mnt

# Create subvolumes. See [ArchWiki: Btrfs#Subvolumes](https://wiki.archlinux.org/title/Btrfs#Subvolumes).
btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
btrfs subvolume create /mnt/@log
btrfs subvolume create /mnt/@cache
btrfs subvolume create /mnt/@snapshots

# Unmount and remount with subvolumes. See [ArchWiki: Btrfs#Mount options](https://wiki.archlinux.org/title/Btrfs#Mount_options).
umount /mnt

# For encrypted:
mount -o subvol=@,compress=zstd,noatime /dev/mapper/cryptroot /mnt

# For unencrypted:
mount -o subvol=@,compress=zstd,noatime /dev/sdX2 /mnt

# Create mount point directories
mkdir -p /mnt/{home,var/log,var/cache,boot,.snapshots}

# Mount all subvolumes
# For encrypted:
mount -o subvol=@home,compress=zstd,noatime /dev/mapper/cryptroot /mnt/home
mount -o subvol=@log,compress=zstd,noatime /dev/mapper/cryptroot /mnt/var/log
mount -o subvol=@cache,compress=zstd,noatime /dev/mapper/cryptroot /mnt/var/cache
mount -o subvol=@snapshots,compress=zstd,noatime /dev/mapper/cryptroot /mnt/.snapshots

# For unencrypted:
mount -o subvol=@home,compress=zstd,noatime /dev/sdX2 /mnt/home
mount -o subvol=@log,compress=zstd,noatime /dev/sdX2 /mnt/var/log
mount -o subvol=@cache,compress=zstd,noatime /dev/sdX2 /mnt/var/cache
mount -o subvol=@snapshots,compress=zstd,noatime /dev/sdX2 /mnt/.snapshots

# Mount EFI boot partition. See [ArchWiki: EFI system partition](https://wiki.archlinux.org/title/EFI_system_partition).
mount /dev/sdX1 /mnt/boot

# Format and enable swap (if created). See [ArchWiki: Swap](https://wiki.archlinux.org/title/Swap).
# For encrypted swap:
mkswap /dev/mapper/cryptswap
swapon /dev/mapper/cryptswap

# For unencrypted swap:
mkswap /dev/sdX3
swapon /dev/sdX3
```

## Verification

```bash
btrfs subvolume list /mnt # Verify subvolumes. See [ArchWiki: Btrfs#Subvolumes](https://wiki.archlinux.org/title/Btrfs#Subvolumes).
mount | grep /mnt # Verify mounts. See [ArchWiki: File systems#Mounting](https://wiki.archlinux.org/title/File_systems#Mounting).
lsblk -f # Verify all mounted filesystems. See [ArchWiki: lsblk](https://wiki.archlinux.org/title/Lsblk).
```

**NEXT:** `core-installation.md`

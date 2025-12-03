# Step: Btrfs Filesystem Creation

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Root partition ready (encrypted or unencrypted)

## Commands

```bash
# Format with Btrfs
# For encrypted:
mkfs.btrfs -L "Arch Linux" /dev/mapper/cryptroot

# For unencrypted:
mkfs.btrfs -L "Arch Linux" /dev/sdX2

# Mount temporarily
# For encrypted:
mount /dev/mapper/cryptroot /mnt

# For unencrypted:
mount /dev/sdX2 /mnt

# Create subvolumes
btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
btrfs subvolume create /mnt/@log
btrfs subvolume create /mnt/@cache
btrfs subvolume create /mnt/@snapshots

# Unmount and remount with subvolumes
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

# Mount EFI boot partition
mount /dev/sdX1 /mnt/boot

# Format and enable swap (if created)
# For encrypted swap:
mkswap /dev/mapper/cryptswap
swapon /dev/mapper/cryptswap

# For unencrypted swap:
mkswap /dev/sdX3
swapon /dev/sdX3
```

## Verification

```bash
btrfs subvolume list /mnt
mount | grep /mnt
lsblk -f
```

**NEXT:** `00-core-installation.md`

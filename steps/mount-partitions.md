# Step: Mount Partitions (Non-Btrfs)

**Purpose:** Mount partitions for installation (if not using Btrfs). For a deeper understanding of filesystem mounting, refer to the [ArchWiki on File systems#Mounting](https://wiki.archlinux.org/title/File_systems#Mounting).

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Disk partitioned, filesystem ready

## Commands

```bash
# Create filesystem on root partition. See [ArchWiki on Ext4](https://wiki.archlinux.org/title/Ext4) and [ArchWiki on XFS](https://wiki.archlinux.org/title/XFS).
# For ext4:
mkfs.ext4 -L "Arch Linux" /dev/sdX2

# For xfs:
mkfs.xfs -L "Arch Linux" /dev/sdX2

# Mount root partition. See [ArchWiki on Mount](https://wiki.archlinux.org/title/Mount).
mount /dev/sdX2 /mnt

# Format and mount EFI partition (if not already formatted). See [ArchWiki: EFI system partition](https://wiki.archlinux.org/title/EFI_system_partition).
mkfs.fat -F32 /dev/sdX1
mount /dev/sdX1 /mnt/boot

# Enable swap (if created). See [ArchWiki: Swap](https://wiki.archlinux.org/title/Swap).
mkswap /dev/sdX3
swapon /dev/sdX3
```

## Verification

```bash
lsblk -f # See [ArchWiki: lsblk](https://wiki.archlinux.org/title/Lsblk).
mount | grep /mnt # See [ArchWiki: File systems#Mounting](https://wiki.archlinux.org/title/File_systems#Mounting).
```

**NEXT:** `core-installation.md`

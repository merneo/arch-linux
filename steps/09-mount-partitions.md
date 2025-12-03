# Step: Mount Partitions (Non-Btrfs)

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Disk partitioned, filesystem ready

## Commands

```bash
# Create filesystem on root partition
# For ext4:
mkfs.ext4 -L "Arch Linux" /dev/sdX2

# For xfs:
mkfs.xfs -L "Arch Linux" /dev/sdX2

# Mount root partition
mount /dev/sdX2 /mnt

# Format and mount EFI partition (if not already formatted)
mkfs.fat -F32 /dev/sdX1
mount /dev/sdX1 /mnt/boot

# Enable swap (if created)
mkswap /dev/sdX3
swapon /dev/sdX3
```

## Verification

```bash
lsblk -f
mount | grep /mnt
```

**NEXT:** `00-core-installation.md`

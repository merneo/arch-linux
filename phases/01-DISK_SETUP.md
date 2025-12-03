# Phase 01: Disk Setup

**Purpose:** Configure disk partitions, encryption, and filesystem

**Prerequisites:**
- Booted from Arch Linux Live USB
- Disk identified

**Steps (execute in order):**

1. **[Disk Partitioning](../steps/06-disk-partitioning.md)**
   - Create EFI, root, and swap partitions
   - Choose: Single boot or Dual boot layout

2. **[LUKS Encryption](../steps/07-luks-encryption.md)** (if using encryption)
   - Encrypt root and swap partitions
   - Set strong passphrase

3. **[Btrfs Filesystem](../steps/08-btrfs-filesystem.md)** (if using Btrfs)
   - Create Btrfs on encrypted or unencrypted partition
   - Create subvolumes: @, @home, @log, @cache, @snapshots

4. **[Mount Partitions](../steps/09-mount-partitions.md)** (if not using Btrfs)
   - Mount partitions for installation (ext4, xfs, etc.)

## Verification

```bash
# Check partitions
lsblk -f

# Check mounts
mount | grep /mnt
```

## Next Phase

**[Phase 02: System Install](02-SYSTEM_INSTALL.md)**

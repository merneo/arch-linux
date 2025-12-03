# Phase 01: Disk Setup

**Purpose:** Configure disk partitions, encryption, and filesystem. This phase is critical for preparing your storage device according to your security, performance, and organizational needs. For detailed background, refer to the [ArchWiki on Partitioning](https://wiki.archlinux.org/title/Partitioning), [dm-crypt (disk encryption)](https://wiki.archlinux.org/title/Dm-crypt), and [Btrfs (filesystem)](https://wiki.archlinux.org/title/Btrfs).

**Prerequisites:**
- Booted from Arch Linux Live USB
- Disk identified

**Steps (execute in order):**

1. **[Disk Partitioning](../steps/disk-partitioning.md)**
   - Create EFI, root, and swap partitions
   - Choose: Single boot or Dual boot layout

2. **[LUKS Encryption](../steps/luks-encryption.md)** (if using encryption)
   - Encrypt root and swap partitions
   - Set strong passphrase

3. **[Btrfs Filesystem](../steps/btrfs-filesystem.md)** (if using Btrfs)
   - Create Btrfs on encrypted or unencrypted partition
   - Create subvolumes: @, @home, @log, @cache, @snapshots

4. **[Mount Partitions](../steps/mount-partitions.md)** (if not using Btrfs)
   - Mount partitions for installation (ext4, xfs, etc.)

## Verification

```bash
# Check partitions
lsblk -f

# Check mounts
mount | grep /mnt
```

---

## Navigation

**Previous:** **[← Phase 00: Preparation](PREPARATION.md)**

**Next:** **[Phase 02: System Install →](SYSTEM_INSTALL.md)**

---

**Back to:** [Main Index](../INDEX.md) | [Repository Root](../README.md)

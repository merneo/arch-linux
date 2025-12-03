# Step: Disk Partitioning

**Purpose:** Create disk partitions for Arch Linux installation. For a comprehensive understanding of disk partitioning, refer to the [ArchWiki on Partitioning](https://wiki.archlinux.org/title/Partitioning) and the utility [cfdisk](https://wiki.archlinux.org/title/Cfdisk).

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Booted from Arch Linux Live USB, disk identified

## Commands

```bash
# Identify disk. For details on lsblk, see [ArchWiki: lsblk](https://wiki.archlinux.org/title/Lsblk).
lsblk

# Launch cfdisk (replace /dev/sdX with your disk). For NVMe drives, adjust accordingly.
# Select 'gpt' (GUID Partition Table) when asked. See [ArchWiki: GUID Partition Table](https://wiki.archlinux.org/title/Partitioning#GUID_Partition_Table).
cfdisk /dev/sdX
# For NVMe: cfdisk /dev/nvme0n1
# Select: gpt (partition table type)
```

**In cfdisk:**

**Single Boot:**
- Create EFI System Partition: 512M, type EFI System. See [ArchWiki: EFI system partition](https://wiki.archlinux.org/title/EFI_system_partition).
- Create Linux Root Partition: 90% of disk. See [ArchWiki: File systems](https://wiki.archlinux.org/title/File_systems).
- Create Swap Partition: 10% of disk (optional), type Linux swap. See [ArchWiki: Swap](https://wiki.archlinux.org/title/Swap).

**Dual Boot:** For a complete guide on dual-booting with Windows, refer to the [ArchWiki on Dual boot with Windows](https://wiki.archlinux.org/title/Dual_boot_with_Windows).
- Use free space or resize Windows partition
- Create Linux Root Partition: available space
- Create Swap Partition: 8G minimum (optional), type Linux swap
- Note: EFI partition should already exist (created by Windows). Verify its type is FAT32.

**Write changes:** [Write] → type `yes` → [Quit]

```bash
# Verify partitions. Forcing kernel to re-read partition table. See [man partprobe](https://man.archlinux.org/man/partprobe.8).
partprobe /dev/sdX
lsblk -f # Verify with [ArchWiki: lsblk](https://wiki.archlinux.org/title/Lsblk).
```

**NEXT:** `07-luks-encryption.md` (if using encryption) or `08-btrfs-filesystem.md` (if using Btrfs) or `09-mount-partitions.md` (if using other filesystem)

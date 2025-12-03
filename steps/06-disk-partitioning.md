# Step: Disk Partitioning

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Booted from Arch Linux Live USB, disk identified

## Commands

```bash
# Identify disk
lsblk

# Launch cfdisk (replace /dev/sdX with your disk)
cfdisk /dev/sdX
# For NVMe: cfdisk /dev/nvme0n1
# Select: gpt (partition table type)
```

**In cfdisk:**

**Single Boot:**
- Create EFI System Partition: 512M, type EFI System
- Create Linux Root Partition: 90% of disk
- Create Swap Partition: 10% of disk (optional), type Linux swap

**Dual Boot:**
- Use free space or resize Windows partition
- Create Linux Root Partition: available space
- Create Swap Partition: 8G minimum (optional), type Linux swap
- Note: EFI partition should already exist (created by Windows)

**Write changes:** [Write] → type `yes` → [Quit]

```bash
# Verify partitions
partprobe /dev/sdX
lsblk -f
```

**NEXT:** `07-luks-encryption.md` (if using encryption) or `08-btrfs-filesystem.md` (if using Btrfs) or `09-mount-partitions.md` (if using other filesystem)

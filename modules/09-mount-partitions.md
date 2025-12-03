# Module: Mount Partitions (Non-Btrfs)

**Purpose:** Mount partitions for installation (if not using Btrfs)

**Prerequisites:**
- Disk partitioned (module `06-disk-partitioning.md`)
- Filesystem created on root partition (ext4, xfs, etc.)

**Time:** 2-3 minutes

**ENVIRONMENT:** Live USB (root@archiso)

---

## Step 1: Create Filesystem on Root Partition

**For ext4:**
```bash
mkfs.ext4 -L "Arch Linux" /dev/sdX2
```

**For xfs:**
```bash
mkfs.xfs -L "Arch Linux" /dev/sdX2
```

---

## Step 2: Mount Root Partition

```bash
mount /dev/sdX2 /mnt
```

---

## Step 3: Mount EFI Boot Partition

```bash
# Format EFI partition (if not already formatted)
mkfs.fat -F32 /dev/sdX1

# Mount EFI partition
mount /dev/sdX1 /mnt/boot
```

---

## Step 4: Enable Swap (if created)

```bash
mkswap /dev/sdX3
swapon /dev/sdX3
```

---

## Step 5: Verify Mounts

```bash
lsblk -f

# Should show:
# sdX
# ├─sdX1  vfat   FAT32  /mnt/boot
# ├─sdX2  ext4   Arch Linux  /mnt
# └─sdX3  swap   [SWAP]
```

---

**SUCCESS:** Partitions mounted and ready for installation

**Next:**
- `00-core-installation.md` - Install base system

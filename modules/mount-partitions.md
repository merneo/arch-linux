# Module: Mount Partitions (Non-Btrfs)

**Purpose:** Mount partitions for installation (if not using Btrfs). This step prepares the partitions for the base system installation by making them accessible within the live environment. For a deeper understanding of filesystem mounting, refer to the [ArchWiki on File systems#Mounting](https://wiki.archlinux.org/title/File_systems#Mounting).

**Prerequisites:**
- Disk partitioned (module `disk-partitioning.md`)
- Filesystem created on root partition (ext4, xfs, etc.)

**Time:** 2-3 minutes

**ENVIRONMENT:** Live USB (root@archiso)

---

## Step 1: Create Filesystem on Root Partition

This step formats the designated root partition with a chosen filesystem. `ext4` is a journaling filesystem and is a common default for Linux, while `xfs` is known for its performance with large files and directories. For more details on these filesystems, refer to the [ArchWiki on Ext4](https://wiki.archlinux.org/title/Ext4) and [ArchWiki on XFS](https://wiki.archlinux.org/title/XFS).

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

The `mount` command attaches the filesystem on the specified device (your root partition) to the filesystem tree at the specified mount point (`/mnt`). For detailed information on the `mount` command and its options, refer to the [ArchWiki on Mount](https://wiki.archlinux.org/title/Mount).

```bash
mount /dev/sdX2 /mnt
```

---

## Step 3: Mount EFI Boot Partition

The [EFI system partition (ESP)](https://wiki.archlinux.org/title/EFI_system_partition) is a mandatory partition for UEFI systems, containing boot loaders and applications. It must be formatted as FAT32. The `mkfs.fat` command creates a FAT filesystem.

```bash
# Format EFI partition (if not already formatted)
mkfs.fat -F32 /dev/sdX1

# Mount EFI partition
mount /dev/sdX1 /mnt/boot
```

---

## Step 4: Enable Swap (if created)

The [swap partition](https://wiki.archlinux.org/title/Swap) provides virtual memory. The `mkswap` command initializes a Linux swap area on a device, and `swapon` enables devices for paging and swapping.

```bash
mkswap /dev/sdX3
swapon /dev/sdX3
```

---

## Step 5: Verify Mounts

The `lsblk -f` command lists block devices, their filesystems, labels, UUIDs, and mountpoints. This is essential for verifying that all partitions are correctly mounted. Refer to the [ArchWiki on lsblk](https://wiki.archlinux.org/title/Lsblk) for more details.

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
- `core-installation.md` - Install base system

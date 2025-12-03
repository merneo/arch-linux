# Module: Btrfs Filesystem Creation

**Purpose:** Create Btrfs filesystem with subvolumes

**Prerequisites:**
- Root partition ready (encrypted or unencrypted)
  - If encrypted: LUKS container opened (module `07-luks-encryption.md`)
  - If unencrypted: Partition identified (e.g., `/dev/sdX2`)

**Time:** 5-10 minutes

---

## Step 1: Format with Btrfs

**For encrypted partition:**
```bash
mkfs.btrfs -L "Arch Linux" /dev/mapper/cryptroot
```

**For unencrypted partition:**
```bash
mkfs.btrfs -L "Arch Linux" /dev/sdX2
```

---

## Step 2: Mount Btrfs Temporarily

**For encrypted:**
```bash
mount /dev/mapper/cryptroot /mnt
```

**For unencrypted:**
```bash
mount /dev/sdX2 /mnt
```

---

## Step 3: Create Btrfs Subvolumes

```bash
# Create @ subvolume (root filesystem)
btrfs subvolume create /mnt/@

# Create @home subvolume (user home directories)
btrfs subvolume create /mnt/@home

# Create @log subvolume (system logs)
btrfs subvolume create /mnt/@log

# Create @cache subvolume (package cache)
btrfs subvolume create /mnt/@cache

# Create @snapshots subvolume (snapshot storage)
btrfs subvolume create /mnt/@snapshots
```

**Verify subvolumes:**
```bash
btrfs subvolume list /mnt

# Should show:
# ID 256 gen X top level 5 path @
# ID 257 gen X top level 5 path @home
# ID 258 gen X top level 5 path @log
# ID 259 gen X top level 5 path @cache
# ID 260 gen X top level 5 path @snapshots
```

---

## Step 4: Unmount and Remount with Subvolumes

```bash
# Unmount root
umount /mnt

# Mount @ subvolume as root with compression
# For encrypted:
mount -o subvol=@,compress=zstd,noatime /dev/mapper/cryptroot /mnt

# For unencrypted:
mount -o subvol=@,compress=zstd,noatime /dev/sdX2 /mnt
```

---

## Step 5: Create Mount Point Directories

```bash
mkdir -p /mnt/{home,var/log,var/cache,boot,.snapshots}
```

---

## Step 6: Mount All Btrfs Subvolumes

**For encrypted:**
```bash
mount -o subvol=@home,compress=zstd,noatime /dev/mapper/cryptroot /mnt/home
mount -o subvol=@log,compress=zstd,noatime /dev/mapper/cryptroot /mnt/var/log
mount -o subvol=@cache,compress=zstd,noatime /dev/mapper/cryptroot /mnt/var/cache
mount -o subvol=@snapshots,compress=zstd,noatime /dev/mapper/cryptroot /mnt/.snapshots
```

**For unencrypted:**
```bash
mount -o subvol=@home,compress=zstd,noatime /dev/sdX2 /mnt/home
mount -o subvol=@log,compress=zstd,noatime /dev/sdX2 /mnt/var/log
mount -o subvol=@cache,compress=zstd,noatime /dev/sdX2 /mnt/var/cache
mount -o subvol=@snapshots,compress=zstd,noatime /dev/sdX2 /mnt/.snapshots
```

---

## Step 7: Mount EFI Boot Partition

```bash
# Mount EFI partition (replace /dev/sdX1 with your EFI partition)
mount /dev/sdX1 /mnt/boot
```

---

## Step 8: Format and Enable Swap (if created)

**For encrypted swap:**
```bash
mkswap /dev/mapper/cryptswap
swapon /dev/mapper/cryptswap
```

**For unencrypted swap:**
```bash
mkswap /dev/sdX3
swapon /dev/sdX3
```

---

## Step 9: Verify All Mounts

```bash
lsblk -f

# Should show all mounted filesystems
```

---

**SUCCESS:** Btrfs filesystem with subvolumes created and mounted

**Next:**
- `09-mount-partitions.md` - Verify mount setup (if not using Btrfs)
- `00-core-installation.md` - Install base system

# Module: Core System Installation

**Purpose:** Install base Arch Linux system using pacstrap. This is the CORE module - only installs the system, nothing else. For a general overview of the installation process, refer to the [ArchWiki Installation Guide](https://wiki.archlinux.org/title/Installation_guide). For details on `pacstrap`, see [ArchWiki: pacstrap](https://wiki.archlinux.org/title/Pacstrap).

**Prerequisites:**
- Booted from Arch Linux Live USB
- Network connection established
- Disk partitions created and mounted at `/mnt` (use `disk-partitioning.md` if needed)

**Time:** 20-40 minutes

**ENVIRONMENT:** Live USB (root@archiso)

**Note:** This module ONLY installs the base system. All configuration (locale, users, GRUB, etc.) is in separate modules.

---

## Prerequisites Checklist

Before starting this module, verify:

- [ ] Booted from Arch Linux Live USB
- [ ] Network connection active (test with `ping archlinux.org`). For more on network configuration, see [ArchWiki: Network configuration](https://wiki.archlinux.org/title/Network_configuration).
- [ ] Disk partitions created and mounted at `/mnt` (check with `lsblk` for device listing and `mount | grep /mnt` for mounted filesystems). Refer to [ArchWiki: Partitioning](https://wiki.archlinux.org/title/Partitioning) and [ArchWiki: File systems#Mounting](https://wiki.archlinux.org/title/File_systems#Mounting).
- [ ] You are in Live USB environment (not chroot)

---

## Step 1: Update Pacman Mirror List (Optional)

For more information on pacman mirrors and `reflector`, refer to the [ArchWiki on Mirrors](https://wiki.archlinux.org/title/Mirrors) and [ArchWiki on reflector](https://wiki.archlinux.org/title/Reflector).

```bash
# Backup original mirrorlist
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup

# Sort mirrors by speed using reflector
reflector --country US,Germany,France \
  --age 12 \
  --protocol https \
  --sort rate \
  --save /etc/pacman.d/mirrorlist

# Wait 10-20 seconds for reflector to test mirrors
```

---

## Step 2: Install Base System

For a detailed explanation of `pacstrap` and essential packages, refer to the [ArchWiki Installation Guide#Install essential packages](https://wiki.archlinux.org/title/Installation_guide#Install_essential_packages).

```bash
pacstrap /mnt \
  base \
  base-devel \
  linux \
  linux-firmware \
  linux-headers \
  btrfs-progs \
  dosfstools \
  e2fsprogs \
  networkmanager \
  vim \
  nano \
  git \
  sudo \
  man-db \
  man-pages
```

**Package breakdown and further reading:**
- `base`: The minimal package set to boot an Arch Linux system. See [ArchWiki: base group](https://wiki.archlinux.org/title/Pacstrap#Package_groups).
- `base-devel`: Required for building packages from AUR. See [ArchWiki: base-devel](https://wiki.archlinux.org/title/Arch_User_Repository#Prerequisites).
- `linux`: The Linux kernel.
- `linux-firmware`: Firmware for various hardware. See [ArchWiki: Microcode](https://wiki.archlinux.org/title/Microcode).
- `linux-headers`: Kernel headers required for building external kernel modules (e.g., some drivers).
- `btrfs-progs`: Utilities for managing Btrfs filesystems. See [ArchWiki: Btrfs](https://wiki.archlinux.org/title/Btrfs).
- `dosfstools`: Utilities for managing FAT filesystems (e.g., for EFI partition).
- `e2fsprogs`: Utilities for managing ext2/ext3/ext4 filesystems.
- `networkmanager`: A daemon for managing network connections. See [ArchWiki: NetworkManager](https://wiki.archlinux.org/title/NetworkManager).
- `vim`: A highly configurable text editor. See [ArchWiki: Vim](https://wiki.archlinux.org/title/Vim).
- `nano`: A simple, user-friendly text editor. See [ArchWiki: Nano](https://wiki.archlinux.org/title/Nano).
- `git`: A distributed version control system. See [ArchWiki: Git](https://wiki.archlinux.org/title/Git).
- `sudo`: Allows a permitted user to execute a command as the superuser or another user. See [ArchWiki: Sudo](https://wiki.archlinux.org/title/Sudo).
- `man-db` / `man-pages`: Provides manual pages for commands.

**Wait for installation** (20-40 minutes depending on internet speed)

---

## Step 3: Generate Fstab

The `genfstab` utility is used to generate an `fstab` (filesystem table) entry for the mounted filesystems. This file is crucial for the system to know which partitions to mount at boot. For details, refer to the [ArchWiki on fstab](https://wiki.archlinux.org/title/Fstab) and [ArchWiki: genfstab](https://wiki.archlinux.org/title/Genfstab).

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

---

## Step 4: Verify Installation

```bash
# Check that critical files were installed
ls /mnt/bin /mnt/etc /mnt/usr

# Should show directories with files
```

---

**SUCCESS:** Base system installed to `/mnt`

## Verification

Run these commands to verify installation:

```bash
# Check that critical directories exist
ls -la /mnt/bin /mnt/etc /mnt/usr

# Should show directories with files (not empty)

# Check fstab was generated
cat /mnt/etc/fstab

# Should show mounted partitions with UUIDs
```

**Next:** Use other modules to configure the system:
- `chroot.md` - Enter chroot environment
- `locale.md` - Set locale and timezone
- `user-creation.md` - Create user account
- `grub.md` - Install GRUB bootloader
- etc.

---

## Troubleshooting

For more general troubleshooting tips during installation, refer to the [ArchWiki Installation Guide#Troubleshooting](https://wiki.archlinux.org/title/Installation_guide#Troubleshooting) section.

### Problem: pacstrap fails with "failed to retrieve files"
**Solution:**
1. Check internet connection: `ping archlinux.org`
2. Update mirrorlist: `reflector --latest 20 --sort rate --save /etc/pacman.d/mirrorlist`
3. Retry installation

### Problem: "target is busy" or mount errors
**Solution:**
1. Check if partitions are already mounted: `mount | grep /mnt`
2. Unmount if needed: `umount -R /mnt`
3. Verify partitions exist: `lsblk`

### Problem: Installation is very slow
**Solution:**
1. Update mirrorlist with faster mirrors: `reflector --country US,Germany --latest 20 --sort rate --save /etc/pacman.d/mirrorlist`
2. Check network speed: `speedtest-cli` (if installed)

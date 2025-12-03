# Core Installation

**Purpose:** Install base Arch Linux system and set root password. This is the foundational step of setting up your Arch Linux environment. For a comprehensive overview of the core installation process, refer to the [ArchWiki Installation Guide#Install essential packages](https://wiki.archlinux.org/title/Installation_guide#Install_essential_packages).

**This is the CORE module - only installs the system, nothing else.**

**Prerequisites:**
- Booted from Arch Linux Live USB
- Network connection established
- Disk partitions created and mounted at `/mnt`



---

## Core Installation Steps

Follow these steps in order:

1. **[Install Base System](core-installation.md#install-base-system)** - Install base Arch Linux system (pacstrap)
2. **[Enter Chroot](core-installation.md#enter-chroot)** - Enter chroot environment
3. **[Set Root Password](core-installation.md#set-root-password)** - Set root password

---

## Install Base System

**Time:** 20-40 minutes

**ENVIRONMENT:** Live USB (root@archiso)

**Note:** This step ONLY installs the base system. All configuration (locale, users, GRUB, etc.) is in post-installation steps.

### Prerequisites Checklist

Before starting, verify:

- [ ] Booted from Arch Linux Live USB
- [ ] Network connection active (test with `ping archlinux.org`)
- [ ] Disk partitions created and mounted at `/mnt` (check with `lsblk` and `mount | grep /mnt`)

### Step 1: Update Pacman Mirror List (Optional)

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

### Step 2: Install Base System

This uses `pacstrap` to install the base packages to the new root. For more info, refer to [ArchWiki: pacstrap](https://wiki.archlinux.org/title/Pacstrap) and [ArchWiki: Installation guide#Install essential packages](https://wiki.archlinux.org/title/Installation_guide#Install_essential_packages).

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
  network-manager-applet \
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

### Step 3: Generate Fstab

The `genfstab` utility is used to generate an `fstab` (filesystem table) entry for the mounted filesystems. This file is crucial for the system to know which partitions to mount at boot. For details, refer to the [ArchWiki on fstab](https://wiki.archlinux.org/title/Fstab) and [ArchWiki: genfstab](https://wiki.archlinux.org/title/Genfstab).

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### Step 4: Verify Installation

```bash
# Check that critical files were installed
ls /mnt/bin /mnt/etc /mnt/usr

# Should show directories with files
```

**SUCCESS:** Base system installed to `/mnt`

**Next:** [Enter Chroot](core-installation.md#enter-chroot)

---

## Enter Chroot

**Time:** 1 minute

**ENVIRONMENT:** Live USB (root@archiso) → Chroot (root@archiso /)#

### Enter Chroot

```bash
arch-chroot /mnt
```

**Prompt changes to:**
```
[root@archiso /]#
```

**SUCCESS:** You are now inside the new Arch system

**Note:** All following commands run inside chroot until you type `exit`. For more details, refer to the [ArchWiki on arch-chroot](https://wiki.archlinux.org/title/Arch-chroot).

**Next:** [Set Root Password](core-installation.md#set-root-password)

---

## Set Root Password

**Time:** 1 minute

**ENVIRONMENT:** Chroot (root@archiso /)#

### Security Notes

- **Root password:** Use a strong password (minimum 12 characters, mix of letters, numbers, symbols). For best practices, consult a guide on [Creating a strong password](https://www.us-cert.gov/ncas/tips/ST04-002).
- **Never share your root password** - root has full system access
- **Remember your password** - you'll need it for system recovery and maintenance

### Set Root Password

The `passwd` command is used to set or change passwords for user accounts. For more details, refer to the [ArchWiki on passwd](https://wiki.archlinux.org/title/Passwd).

```bash
passwd
```

**Prompt:**
```
New password:
```

**Enter strong root password** (this is for system recovery, not daily use)

**Retype password** to confirm.

---

**SUCCESS:** Root password set

**Core installation complete!**

**Next:** Choose post-installation configuration steps:
- [Post-Installation Configuration](post-installation.md)

---

**Back to:** [Home](HOME.md) | **Next:** [Post-Installation Configuration](post-installation.md)

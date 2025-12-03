# Core Installation

**Purpose:** Install base Arch Linux system and set root password

**This is the CORE module - only installs the system, nothing else.**

**Prerequisites:**
- Booted from Arch Linux Live USB
- Network connection established
- Disk partitions created and mounted at `/mnt`

**Note:** This is the vendor-neutral core version. For Intel or AMD specific versions, see:
- [Intel Version](../core-intel/wiki/02-CORE-INSTALLATION.md) - Includes `intel-ucode` and Intel graphics drivers
- [AMD Version](../core-amd/wiki/02-CORE-INSTALLATION.md) - Includes `amd-ucode` and AMD graphics drivers

---

## Core Installation Steps

Follow these steps in order:

1. **[Install Base System](#install-base-system)** - Install base Arch Linux system (pacstrap)
2. **[Enter Chroot](#enter-chroot)** - Enter chroot environment
3. **[Set Root Password](#set-root-password)** - Set root password

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

**Wait for installation** (20-40 minutes depending on internet speed)

### Step 3: Generate Fstab

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

**Next:** [Enter Chroot](#enter-chroot)

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

**Note:** All following commands run inside chroot until you type `exit`

**Next:** [Set Root Password](#set-root-password)

---

## Set Root Password

**Time:** 1 minute

**ENVIRONMENT:** Chroot (root@archiso /)#

### Security Notes

- **Root password:** Use a strong password (minimum 12 characters, mix of letters, numbers, symbols)
- **Never share your root password** - root has full system access
- **Remember your password** - you'll need it for system recovery and maintenance

### Set Root Password

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
- [Post-Installation Configuration](03-POST-INSTALLATION.md)

---

**Back to:** [Home](00-HOME.md) | **Next:** [Post-Installation Configuration](03-POST-INSTALLATION.md)

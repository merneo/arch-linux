# Module: Core System Installation

**Purpose:** Install base Arch Linux system using pacstrap. This is the CORE module - only installs the system, nothing else.

**Prerequisites:**
- Booted from Arch Linux Live USB
- Network connection established
- Disk partitions created and mounted at `/mnt` (use `06-disk-partitioning.md` if needed)

**Time:** 20-40 minutes

**ENVIRONMENT:** Live USB (root@archiso)

**Note:** This module ONLY installs the base system. All configuration (locale, users, GRUB, etc.) is in separate modules.

---

## Prerequisites Checklist

Before starting this module, verify:

- [ ] Booted from Arch Linux Live USB
- [ ] Network connection active (test with `ping archlinux.org`)
- [ ] Disk partitions created and mounted at `/mnt` (check with `lsblk` and `mount | grep /mnt`)
- [ ] You are in Live USB environment (not chroot)

---

## Step 1: Update Pacman Mirror List (Optional)

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

---

## Step 3: Generate Fstab

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
- `01-chroot.md` - Enter chroot environment
- `02-locale.md` - Set locale and timezone
- `03-root-password.md` - Set root password
- `04-user-creation.md` - Create user account
- `05-grub.md` - Install GRUB bootloader
- etc.

---

## Troubleshooting

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

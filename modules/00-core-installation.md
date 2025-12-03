# Module: Core System Installation

**Purpose:** Install base Arch Linux system using pacstrap (assumes you're already booted from USB)

**Prerequisites:**
- Booted from Arch Linux Live USB
- Network connection established
- Disk partitions created and mounted at `/mnt`

**Time:** 20-40 minutes

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

**Next:** Use other modules to configure the system:
- `01-chroot.md` - Enter chroot environment
- `02-locale.md` - Set locale and timezone
- `03-root-password.md` - Set root password
- `04-user-creation.md` - Create user account
- `05-grub.md` - Install GRUB bootloader
- etc.

# Complete Arch Linux Installation Guide

**Purpose:** Complete installation guide with all steps in one document

**This guide combines all installation phases into a single document for easy reference.**

---

## Table of Contents

1. [Pre-Installation Steps](#pre-installation-steps)
2. [Core Installation](#core-installation)
3. [Post-Installation Configuration](#post-installation-configuration)

---

## Pre-Installation Steps

**Purpose:** Prepare disk and filesystem before installing the system

**When to use:** Only if your disk is not already partitioned and mounted at `/mnt`

---

## Menu: Pre-Installation Steps

Choose what you need:

- **[Disk Partitioning](#disk-partitioning)** - Create disk partitions (only if needed)
- **[LUKS Encryption](#luks-encryption)** - Encrypt partitions
- **[Btrfs Filesystem](#btrfs-filesystem)** - Create Btrfs with subvolumes
- **[Mount Partitions](#mount-partitions)** - Mount partitions (non-Btrfs)

---

## Disk Partitioning

**Purpose:** Create disk partitions for Arch Linux installation

**Prerequisites:**
- Booted from Arch Linux Live USB
- Disk identified

**Time:** 10-15 minutes

**Note:** This step is **OPTIONAL**. Only use if your disk is not already partitioned.

### Step 1: Identify Disk Device

```bash
# List all block devices
lsblk

# Identify target disk (usually the largest one, NOT the USB drive)
# USB drives are typically smaller (8-32 GB)
# Note device name: /dev/sda or /dev/nvme0n1
```

### Step 2: Launch cfdisk

```bash
# Open cfdisk for your disk (replace /dev/sdX with your actual device)
cfdisk /dev/sdX

# For NVMe drives:
cfdisk /dev/nvme0n1

# Partition table type should be: gpt
# If asked "Select label type", choose: gpt
```

### Step 3: Create Partitions

**Create EFI System Partition:**
1. Navigate to **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `512M` (minimum 512 MB for EFI) and press Enter
4. Navigate to the newly created partition
5. Press Enter on **[ Type ]** menu option
6. Scroll down to find **EFI System** (type code EF00)
7. Press Enter to select EFI System

**Create Linux Root Partition:**
1. Navigate to remaining **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `70%` (or specific size like `350G`) and press Enter
4. Partition type will default to **Linux filesystem** (correct, do not change)

**Create Swap Partition (Optional):**
1. Navigate to remaining **Free space** row
2. Press Enter on **[ New ]** menu option
3. **Partition size**: Type `5%` (or `8G` minimum) and press Enter
4. Navigate to the newly created partition
5. Press Enter on **[ Type ]** menu option
6. Scroll down to find **Linux swap** (type code 19 or 82)
7. Press Enter to select Linux swap

### Step 4: Write Changes

1. Navigate to **[ Write ]** menu option
2. Press Enter
3. **Confirmation prompt**: `Are you sure you want to write the partition table to disk? (yes or no):`
4. Type exactly: `yes` (lowercase, no quotes)
5. Press Enter
6. Navigate to **[ Quit ]** menu option
7. Press Enter to exit cfdisk

### Step 5: Verify Partitions

```bash
# Force kernel to re-read partition table
partprobe /dev/sdX

# List partitions again
lsblk -f

# Should show:
# sdX
# ├─sdX1  vfat   FAT32  (EFI System)
# ├─sdX2          (Linux root - empty)
# └─sdX3          (Linux swap - empty, optional)
```

**SUCCESS:** Disk partitioned successfully

**Next:** 
- [LUKS Encryption](#luks-encryption) - Encrypt partitions (if needed)
- [Btrfs Filesystem](#btrfs-filesystem) - Create Btrfs filesystem
- [Mount Partitions](#mount-partitions) - Mount partitions (non-Btrfs)
- [Core Installation](02-CORE-INSTALLATION.md) - If partitions are already mounted

---

## LUKS Encryption

**Purpose:** Encrypt disk partitions with LUKS2

**Prerequisites:**
- Disk partitioned (see [Disk Partitioning](#disk-partitioning) above)
- Root partition identified (e.g., `/dev/sdX2`)

**Time:** 10-15 minutes

### Step 1: Encrypt Root Partition

```bash
# Replace /dev/sdX2 with your root partition
cryptsetup luksFormat --type luks2 \
  --cipher aes-xts-plain64 \
  --key-size 512 \
  --pbkdf argon2id \
  --iter-time 5000 \
  /dev/sdX2
```

**Prompt 1: Confirmation**
```
WARNING!
========
This will overwrite data on /dev/sdX2 irrevocably.

Are you sure? (Type 'yes' in capital letters):
```

**Type exactly:** `YES` (uppercase) and press Enter

**Prompt 2: Passphrase**
```
Enter passphrase for /dev/sdX2:
```

**Enter a STRONG passphrase** (minimum 20 characters recommended)

**Prompt 3: Verify passphrase**
```
Verify passphrase:
```

**Re-type the EXACT SAME passphrase** and press Enter

**Wait for completion** (30-60 seconds)

### Step 2: Open Encrypted Root

```bash
# Unlock LUKS container and map to /dev/mapper/cryptroot
cryptsetup open /dev/sdX2 cryptroot

# Enter the passphrase you just created
```

**Verify:**
```bash
ls -la /dev/mapper/

# Should show:
# cryptroot -> ../dm-0
```

### Step 3: Encrypt Swap Partition (Optional)

```bash
# Replace /dev/sdX3 with your swap partition
cryptsetup luksFormat --type luks2 \
  --cipher aes-xts-plain64 \
  --key-size 512 \
  --pbkdf argon2id \
  --iter-time 5000 \
  /dev/sdX3

# Type YES, enter passphrase (can be same as root)

# Open encrypted swap
cryptsetup open /dev/sdX3 cryptswap
```

### Step 4: Verify Encryption

```bash
# Display LUKS header information
cryptsetup luksDump /dev/sdX2 | head -20

# Should show:
# LUKS header information for /dev/sdX2
# Version:        2
# Cipher name:    aes
# Cipher mode:    xts-plain64
# Hash spec:      sha512
# Key Slot 0: ENABLED
```

**SUCCESS:** Partitions encrypted with LUKS2

**Next:**
- [Btrfs Filesystem](#btrfs-filesystem) - Create Btrfs filesystem on encrypted partition
- [Mount Partitions](#mount-partitions) - Mount partitions (non-Btrfs)
- [Core Installation](02-CORE-INSTALLATION.md) - If partitions are already mounted

---

## Btrfs Filesystem

**Purpose:** Create Btrfs filesystem with subvolumes

**Prerequisites:**
- Root partition ready (encrypted or unencrypted)
  - If encrypted: LUKS container opened (see [LUKS Encryption](#luks-encryption) above)
  - If unencrypted: Partition identified (e.g., `/dev/sdX2`)

**Time:** 5-10 minutes

### Step 1: Format with Btrfs

**For encrypted partition:**
```bash
mkfs.btrfs -L "Arch Linux" /dev/mapper/cryptroot
```

**For unencrypted partition:**
```bash
mkfs.btrfs -L "Arch Linux" /dev/sdX2
```

### Step 2: Mount Btrfs Temporarily

**For encrypted:**
```bash
mount /dev/mapper/cryptroot /mnt
```

**For unencrypted:**
```bash
mount /dev/sdX2 /mnt
```

### Step 3: Create Btrfs Subvolumes

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

### Step 4: Unmount and Remount with Subvolumes

```bash
# Unmount root
umount /mnt

# Mount @ subvolume as root with compression
# For encrypted:
mount -o subvol=@,compress=zstd,noatime /dev/mapper/cryptroot /mnt

# For unencrypted:
mount -o subvol=@,compress=zstd,noatime /dev/sdX2 /mnt
```

### Step 5: Create Mount Point Directories

```bash
mkdir -p /mnt/{home,var/log,var/cache,boot,.snapshots}
```

### Step 6: Mount All Btrfs Subvolumes

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

### Step 7: Mount EFI Boot Partition

```bash
# Mount EFI partition (replace /dev/sdX1 with your EFI partition)
mount /dev/sdX1 /mnt/boot
```

### Step 8: Format and Enable Swap (if created)

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

### Step 9: Verify All Mounts

```bash
lsblk -f

# Should show all mounted filesystems
```

**SUCCESS:** Btrfs filesystem with subvolumes created and mounted

**Next:**
- [Core Installation](02-CORE-INSTALLATION.md) - Install base system

---

## Mount Partitions

**Purpose:** Mount partitions for installation (if not using Btrfs)

**Prerequisites:**
- Disk partitioned (see [Disk Partitioning](#disk-partitioning) above)
- Filesystem created on root partition (ext4, xfs, etc.)

**Time:** 2-3 minutes

### Step 1: Create Filesystem on Root Partition

**For ext4:**
```bash
mkfs.ext4 -L "Arch Linux" /dev/sdX2
```

**For xfs:**
```bash
mkfs.xfs -L "Arch Linux" /dev/sdX2
```

### Step 2: Mount Root Partition

```bash
mount /dev/sdX2 /mnt
```

### Step 3: Mount EFI Boot Partition

```bash
# Format EFI partition (if not already formatted)
mkfs.fat -F32 /dev/sdX1

# Mount EFI partition
mount /dev/sdX1 /mnt/boot
```

### Step 4: Enable Swap (if created)

```bash
mkswap /dev/sdX3
swapon /dev/sdX3
```

### Step 5: Verify Mounts

```bash
lsblk -f

# Should show:
# sdX
# ├─sdX1  vfat   FAT32  /mnt/boot
# ├─sdX2  ext4   Arch Linux  /mnt
# └─sdX3  swap   [SWAP]
```

**SUCCESS:** Partitions mounted and ready for installation

**Next:**
- [Core Installation](02-CORE-INSTALLATION.md) - Install base system

---



---

## Core Installation

**Purpose:** Install base Arch Linux system and set root password

**This is the CORE module - only installs the system, nothing else.**

**Prerequisites:**
- Booted from Arch Linux Live USB
- Network connection established
- Disk partitions created and mounted at `/mnt`

---

### Core Installation Steps

Follow these steps in order:

1. **[Install Base System](#install-base-system)** - Install base Arch Linux system (pacstrap)
2. **[Enter Chroot](#enter-chroot)** - Enter chroot environment
3. **[Set Root Password](#set-root-password)** - Set root password

---

## Install Base System

**Time:** 20-40 minutes

**Note:** This step ONLY installs the base system. All configuration (locale, users, GRUB, etc.) is in post-installation steps.

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



---

## Post-Installation Configuration

**Purpose:** Configure system after core installation

**Prerequisites:**
- Core installation completed (see [Core Installation](02-CORE-INSTALLATION.md))
- Inside chroot environment

---

## Menu: Post-Installation Configuration

Choose what you need:

- **[Locale & Timezone](#locale-timezone)** - Set locale and timezone
- **[User Creation](#user-creation)** - Create user account with sudo
- **[GRUB Bootloader](#grub-bootloader)** - Install GRUB (required)
- **[LUKS Auto-Unlock](#luks-auto-unlock)** - Automatic LUKS decryption
- **[NetworkManager](#networkmanager)** - Enable NetworkManager
- **[WiFi Support](#wifi-support)** - WiFi configuration
- **[Bluetooth](#bluetooth)** - Bluetooth support
- **[Audio Server](#audio-server)** - PipeWire audio
- **[Exit & Reboot](#exit-reboot)** - Final step

---

## Locale & Timezone

**Purpose:** Configure system locale and timezone

**Prerequisites:**
- Inside chroot environment

**Time:** 2-3 minutes

### Step 1: Set Timezone

```bash
# Replace with your timezone (examples: Europe/Prague, America/New_York, Asia/Tokyo)
ln -sf /usr/share/zoneinfo/Europe/Prague /etc/localtime

# Generate /etc/adjtime
hwclock --systohc
```

**Verify timezone:**
```bash
timedatectl status
```

### Step 2: Set Locale

```bash
# Uncomment your locale in locale.gen
# For English (US):
sed -i 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen

# For other locales, edit manually:
# nano /etc/locale.gen
# Uncomment desired locale (e.g., cs_CZ.UTF-8, de_DE.UTF-8)

# Generate locales
locale-gen
```

### Step 3: Set System Locale

```bash
# Set default locale
echo "LANG=en_US.UTF-8" > /etc/locale.conf

# Or for other locales:
# echo "LANG=cs_CZ.UTF-8" > /etc/locale.conf
```

**Verify locale:**
```bash
cat /etc/locale.conf
```

**SUCCESS:** Locale and timezone configured

**Next:** Continue with other configuration steps

---

## User Creation

**Purpose:** Create non-root user account with sudo access

**Prerequisites:**
- Inside chroot environment
- Sudo package installed (included in core installation)

**Time:** 2-3 minutes

### Step 1: Create User

```bash
# Replace "username" with your desired username
useradd -m -G wheel,video,audio,storage,input,power,network -s /bin/bash username

# Explanation:
# -m: Create home directory (/home/username)
# -G: Add to supplementary groups:
#     wheel: sudo access
#     video: GPU access
#     audio: sound access
#     storage: removable media access
#     input: input devices
#     power: power management
#     network: network configuration
# -s /bin/bash: Default shell (or use /bin/zsh)
```

### Step 2: Set User Password

```bash
passwd username
```

**Enter password** for the user.

### Step 3: Configure Sudo Access

```bash
EDITOR=nano visudo
```

**Find line:**
```bash
# %wheel ALL=(ALL:ALL) ALL
```

**Uncomment (remove #):**
```bash
%wheel ALL=(ALL:ALL) ALL
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

### Step 4: Verify User

```bash
id username

# Should show:
# uid=1000(username) gid=1000(username) groups=1000(username),wheel,video,audio,storage,input,power,network
```

**SUCCESS:** User created with sudo access

**Next:** Continue with other configuration steps

---

## GRUB Bootloader

**Purpose:** Install and configure GRUB bootloader

**Prerequisites:**
- Inside chroot environment
- EFI partition mounted at `/boot`
- Root partition UUID identified

**Time:** 10-15 minutes

**Note:** This step is **REQUIRED** - system won't boot without GRUB.

### Step 1: Install GRUB

```bash
pacman -S grub efibootmgr os-prober

# Install GRUB to EFI partition
grub-install --target=x86_64-efi \
  --efi-directory=/boot \
  --bootloader-id=GRUB \
  --recheck
```

### Step 2: Get Root Partition UUID

```bash
# Display root partition UUID
blkid /dev/sdX2

# Or filter only UUID value
blkid -s UUID -o value /dev/sdX2
```

**Copy the UUID** (format: `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX`)

### Step 3: Configure GRUB

```bash
nano /etc/default/grub
```

**For standard installation (no encryption):**
```bash
GRUB_CMDLINE_LINUX="root=UUID=<YOUR_UUID>"
GRUB_DISABLE_OS_PROBER=false
```

**For LUKS encryption:**
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<YOUR_UUID>:cryptroot root=/dev/mapper/cryptroot"
GRUB_DISABLE_OS_PROBER=false
```

**For Btrfs subvolume:**
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<YOUR_UUID>:cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@"
GRUB_DISABLE_OS_PROBER=false
```

**Replace `<YOUR_UUID>` with actual UUID from Step 2**

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

### Step 4: Generate GRUB Configuration

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

**Expected output:**
```
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-linux
Found initrd image: /boot/initramfs-linux.img
done
```

**SUCCESS:** GRUB bootloader installed and configured

**Next:** Continue with other configuration steps or [Exit & Reboot](#exit-reboot)

---

## LUKS Auto-Unlock

**Purpose:** Configure automatic LUKS decryption on boot using keyfile

**Prerequisites:**
- Inside chroot environment
- LUKS encryption configured (see [Pre-Installation: LUKS Encryption](01-PRE-INSTALLATION.md#luks-encryption))
- GRUB configured (see [GRUB Bootloader](#grub-bootloader) above)

**Time:** 10-15 minutes

### Step 1: Create Keyfile Directory

```bash
mkdir -p /etc/cryptsetup.d
chmod 700 /etc/cryptsetup.d
```

### Step 2: Generate LUKS Keyfile

```bash
# Generate 512-byte random keyfile
dd if=/dev/urandom of=/etc/cryptsetup.d/root.key bs=512 count=1

# Set strict permissions
chmod 600 /etc/cryptsetup.d/root.key
```

### Step 3: Add Keyfile to LUKS Key Slot

```bash
# Replace /dev/sdX2 with your root partition
cryptsetup luksAddKey /dev/sdX2 /etc/cryptsetup.d/root.key
```

**Enter existing LUKS passphrase** when prompted.

**Verify keyfile was added:**
```bash
cryptsetup luksDump /dev/sdX2 | grep "Key Slot"

# Should show:
# Key Slot 0: ENABLED
# Key Slot 1: ENABLED
```

### Step 4: Add Keyfile to Swap Partition (if encrypted)

```bash
# Replace /dev/sdX3 with your swap partition
cryptsetup luksAddKey /dev/sdX3 /etc/cryptsetup.d/root.key

# Enter existing passphrase
```

### Step 5: Configure Initramfs to Include Keyfile

```bash
nano /etc/mkinitcpio.conf
```

**Find line:**
```bash
FILES=()
```

**Change to:**
```bash
FILES=(/etc/cryptsetup.d/root.key)
```

**Find HOOKS line:**
```bash
HOOKS=(base udev autodetect modconf block filesystems keyboard fsck)
```

**Add `encrypt` hook (MUST be after `block`, before `filesystems`):**
```bash
HOOKS=(base udev autodetect modconf block encrypt filesystems keyboard fsck)
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

### Step 6: Update GRUB for Keyfile

```bash
nano /etc/default/grub
```

**Find line starting with `GRUB_CMDLINE_LINUX=`**

**Add `cryptkey` parameter:**
```bash
GRUB_CMDLINE_LINUX="cryptkey=rootfs:/etc/cryptsetup.d/root.key cryptdevice=UUID=<YOUR_UUID>:cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@"
```

**Replace `<YOUR_UUID>` with your root partition UUID**

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

### Step 7: Configure Encrypted Swap Auto-Unlock

```bash
# Create /etc/crypttab for swap
cat > /etc/crypttab << EOF
# <name>       <device>                             <keyfile>                          <options>
cryptswap      /dev/sdX3                            /etc/cryptsetup.d/root.key         luks
EOF
```

**Replace `/dev/sdX3` with your swap partition**

**Update fstab for encrypted swap:**
```bash
nano /etc/fstab
```

**Find swap line and change to:**
```
/dev/mapper/cryptswap  none  swap  defaults  0  0
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

### Step 8: Rebuild Initramfs and GRUB

```bash
# Regenerate initramfs (includes keyfile now)
mkinitcpio -P

# Regenerate GRUB config
grub-mkconfig -o /boot/grub/grub.cfg
```

### Step 9: Verify Keyfile is in Initramfs

```bash
# List contents of initramfs
lsinitcpio /boot/initramfs-linux.img | grep root.key

# Should show:
# etc/cryptsetup.d/root.key
```

**SUCCESS:** Automatic LUKS decryption configured

**Note:** System will now boot without asking for LUKS passphrase (keyfile is in `/boot` partition)

**Next:** Continue with other configuration steps

---

## NetworkManager

**Purpose:** Enable NetworkManager for network connectivity

**Prerequisites:**
- Inside chroot environment
- NetworkManager installed (included in core installation)

**Time:** 1 minute

### Enable NetworkManager Service

```bash
systemctl enable NetworkManager
```

**Output:**
```
Created symlink /etc/systemd/system/multi-user.target.wants/NetworkManager.service → /usr/lib/systemd/system/NetworkManager.service
```

**SUCCESS:** NetworkManager will start automatically on boot

**After reboot:**
- NetworkManager will automatically connect to wired networks
- Use `nmcli` or `nmtui` to configure WiFi
- Or use NetworkManager GUI applet

**Next:** Continue with other configuration steps

---

## WiFi Support

**Purpose:** Install and configure WiFi support

**Prerequisites:**
- Inside chroot environment
- NetworkManager enabled (see [NetworkManager](#networkmanager) above)

**Time:** 2-3 minutes

### Step 1: Install WiFi Packages

```bash
pacman -S wireless_tools wpa_supplicant dialog
```

**Package breakdown:**
- `wireless_tools` - WiFi utilities
- `wpa_supplicant` - WPA/WPA2 authentication
- `dialog` - TUI dialogs (for WiFi setup)

### Step 2: Verify WiFi Support

**After reboot, use NetworkManager to connect:**

**Command line:**
```bash
nmcli device wifi list
nmcli device wifi connect "SSID" password "password"
```

**Text UI:**
```bash
nmtui
```

**GUI:**
- NetworkManager applet (if desktop environment installed)

**SUCCESS:** WiFi support installed

**Note:** WiFi configuration happens after first boot using NetworkManager

**Next:** Continue with other configuration steps

---

## Bluetooth

**Purpose:** Install and enable Bluetooth support

**Prerequisites:**
- Inside chroot environment

**Time:** 2-3 minutes

### Step 1: Install Bluetooth Packages

```bash
pacman -S bluez bluez-utils
```

**Package breakdown:**
- `bluez` - Bluetooth protocol stack
- `bluez-utils` - bluetoothctl and other tools

### Step 2: Enable Bluetooth Service

```bash
systemctl enable bluetooth
```

**Output:**
```
Created symlink /etc/systemd/system/dbus-org.bluez.service → /usr/lib/systemd/system/bluetooth.service
```

**SUCCESS:** Bluetooth will start automatically on boot

**After reboot:**
- Use `bluetoothctl` to pair devices
- Or use Bluetooth GUI (if desktop environment installed)

**Example usage:**
```bash
bluetoothctl
power on
scan on
pair <device-address>
connect <device-address>
```

**Next:** Continue with other configuration steps

---

## Audio Server

**Purpose:** Install and configure audio server (PipeWire)

**Prerequisites:**
- Inside chroot environment

**Time:** 3-5 minutes

### Step 1: Install PipeWire Audio Stack

```bash
pacman -S \
  pipewire \
  pipewire-alsa \
  pipewire-pulse \
  pipewire-jack \
  wireplumber \
  pavucontrol
```

**Package breakdown:**
- `pipewire` - Modern audio server
- `pipewire-alsa` - ALSA compatibility
- `pipewire-pulse` - PulseAudio compatibility
- `pipewire-jack` - JACK compatibility
- `wireplumber` - Session manager
- `pavucontrol` - Audio control GUI

### Step 2: Enable PipeWire Services

```bash
systemctl --user enable pipewire pipewire-pulse wireplumber
```

**Note:** User services are enabled per-user, not system-wide

**SUCCESS:** Audio server installed

**After reboot and login:**
- Audio should work automatically
- Use `pavucontrol` to control audio settings
- PipeWire provides compatibility with ALSA, PulseAudio, and JACK applications

**Next:** Continue with other configuration steps or [Exit & Reboot](#exit-reboot)

---

## Exit & Reboot

**Purpose:** Exit chroot environment, unmount partitions, and reboot system

**Prerequisites:**
- All configuration steps completed
- System ready for first boot

**Time:** 5 minutes

### Step 1: Exit Chroot Environment

```bash
exit
```

**Prompt changes back to:**
```
root@archiso ~ #
```

**SUCCESS:** You are now back in Arch Linux live USB environment

### Step 2: Unmount All Partitions

```bash
# Unmount all mounted filesystems recursively
umount -R /mnt
```

**If error "target is busy":**
```bash
# Force unmount
umount -lR /mnt
```

### Step 3: Close Encrypted Volumes (if used)

```bash
# Close encrypted swap
cryptsetup close cryptswap

# Close encrypted root
cryptsetup close cryptroot
```

**Verify all closed:**
```bash
ls /dev/mapper/

# Should show only:
# control
```

### Step 4: Reboot System

```bash
reboot
```

**Remove USB drive** when system shuts down (before it boots again).

---

**SUCCESS:** System ready for first boot

**After reboot:**
- GRUB menu appears
- Select Arch Linux
- Enter LUKS passphrase (if not using keyfile)
- Login with created user
- Continue with post-installation configuration

---

**Installation Complete!**

---


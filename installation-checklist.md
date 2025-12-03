# Installation Checklist

**Purpose:** Interactive checklist to track your installation progress. Check off items as you complete them.

**Quick Links:**
- [Main Index](index.md) - Choose your hardware configuration
- [Installation Phases](phases/00-preparation.md) - Detailed phase guides
- [Troubleshooting](troubleshooting.md) - If you encounter problems

---

## Pre-Installation Preparation

### Hardware & Software Requirements
- [ ] Second computer with internet connection (for USB creation)
- [ ] USB flash drive (8 GB minimum)
- [ ] Target computer ready
- [ ] Arch Linux ISO downloaded (or ready to download)
- [ ] Windows installation media (if dual booting)

### Knowledge Requirements
- [ ] Basic Linux command line knowledge
- [ ] Understanding of disk partitioning (or ready to learn)
- [ ] Backup of important data (if installing on existing system)

---

## Phase 00: Preparation

**Time Estimate:** 30-60 minutes  
**Difficulty:** ⭐ Easy

### Create Arch Linux USB
- [ ] Arch Linux ISO downloaded
- [ ] USB drive prepared (backed up if needed)
- [ ] Bootable USB created (Linux/Windows/macOS method)
- [ ] USB tested (booted from USB on target computer)

### Install Windows (Dual Boot Only)
- [ ] Windows installation media ready
- [ ] Windows installed on target computer
- [ ] Windows Fast Startup disabled
- [ ] BitLocker disabled (if was enabled)
- [ ] Free space left for Arch Linux (minimum 20 GB recommended)

### Format Disk (Fresh Install Only)
- [ ] All important data backed up
- [ ] Disk completely wiped (if desired)
- [ ] Ready to proceed with partitioning

**Verification:**
- [ ] Can boot from Arch Linux USB
- [ ] Windows installed and configured (if dual booting)
- [ ] Disk ready for partitioning

---

## Phase 01: Disk Setup

**Time Estimate:** 15-30 minutes  
**Difficulty:** ⭐⭐ Intermediate

### Disk Partitioning
- [ ] Booted from Arch Linux Live USB
- [ ] Network connection established
- [ ] Disk identified (`lsblk`)
- [ ] Partitions created:
  - [ ] EFI System Partition (512 MB, FAT32)
  - [ ] Root partition (size determined)
  - [ ] Swap partition (optional, size determined)
- [ ] Partition layout verified (`lsblk`)

### LUKS Encryption (If Using)
- [ ] Root partition encrypted
- [ ] Swap partition encrypted (if using encrypted swap)
- [ ] Strong passphrase set and remembered
- [ ] LUKS volumes opened for filesystem creation

### Btrfs Filesystem (If Using)
- [ ] Btrfs created on root partition
- [ ] Subvolumes created:
  - [ ] @ (root)
  - [ ] @home
  - [ ] @log
  - [ ] @cache
  - [ ] @snapshots
- [ ] Subvolumes mounted correctly

### Mount Partitions (If Not Using Btrfs)
- [ ] Root partition formatted (ext4/xfs)
- [ ] Root partition mounted at `/mnt`
- [ ] EFI partition mounted at `/mnt/boot`
- [ ] Swap enabled (if using swap)

**Verification:**
```bash
lsblk -f
mount | grep /mnt
```
- [ ] All partitions visible and mounted correctly

---

## Phase 02: System Install

**Time Estimate:** 10-20 minutes (depends on internet speed)  
**Difficulty:** ⭐ Easy

### Core System Installation
- [ ] Network connection active (`ping archlinux.org`)
- [ ] Pacman mirror list updated (optional, recommended)
- [ ] Base system installed (`pacstrap`)
- [ ] fstab generated
- [ ] Installation completed without errors

### Enter Chroot
- [ ] Successfully entered chroot environment
- [ ] Can run commands in chroot
- [ ] Network still works in chroot (if needed)

**Verification:**
```bash
ls -la /mnt/bin /mnt/etc /mnt/usr
arch-chroot /mnt
```
- [ ] System files present
- [ ] Chroot environment working

---

## Phase 03: Basic Configuration

**Time Estimate:** 10-15 minutes  
**Difficulty:** ⭐ Easy

### Locale and Timezone
- [ ] Locale uncommented in `/etc/locale.gen`
- [ ] Locale generated (`locale-gen`)
- [ ] `/etc/locale.conf` configured
- [ ] Timezone set (`timedatectl set-timezone`)
- [ ] Hardware clock set to UTC

### User Creation
- [ ] User account created
- [ ] User added to wheel group (for sudo)
- [ ] Sudo configured (`visudo`)
- [ ] Root password set
- [ ] User password set

**Verification:**
```bash
locale
timedatectl status
id <USERNAME>
```
- [ ] Locale and timezone correct
- [ ] User exists and has sudo access

---

## Phase 04: Bootloader

**Time Estimate:** 10-15 minutes  
**Difficulty:** ⭐⭐ Intermediate

### GRUB Installation
- [ ] GRUB packages installed
- [ ] GRUB installed to EFI partition
- [ ] `/etc/default/grub` configured:
  - [ ] Root UUID set (or cryptdevice for LUKS)
  - [ ] Btrfs subvolume flags (if using Btrfs)
  - [ ] os-prober enabled (if dual booting)
- [ ] GRUB configuration generated
- [ ] GRUB files present in `/boot/EFI/GRUB/`

### LUKS Auto-Unlock (If Using)
- [ ] Keyfile created
- [ ] Keyfile added to LUKS volume
- [ ] Keyfile added to initramfs
- [ ] GRUB configured for keyfile
- [ ] Initramfs regenerated

**Verification:**
```bash
ls -la /boot/grub/grub.cfg
ls -la /boot/EFI/GRUB/
grep "GRUB_CMDLINE_LINUX" /etc/default/grub
```
- [ ] GRUB configuration files exist
- [ ] Configuration correct

---

## Phase 05: Network Configuration

**Time Estimate:** 5-10 minutes  
**Difficulty:** ⭐ Easy

### NetworkManager
- [ ] NetworkManager service enabled
- [ ] NetworkManager service started (after reboot)

### WiFi (If Using)
- [ ] WiFi packages installed
- [ ] WiFi configured (after first boot)

### Bluetooth (If Using)
- [ ] Bluetooth packages installed
- [ ] Bluetooth service enabled

**Verification:**
```bash
systemctl is-enabled NetworkManager
systemctl is-enabled bluetooth
```
- [ ] Services enabled correctly

---

## Phase 06: Audio Configuration

**Time Estimate:** 5-10 minutes  
**Difficulty:** ⭐ Easy

### PipeWire Audio
- [ ] PipeWire packages installed
- [ ] PipeWire services enabled (user services)
- [ ] Audio working (test after first boot)

**Verification:**
```bash
systemctl --user is-enabled pipewire
systemctl --user is-enabled pipewire-pulse
```
- [ ] Services enabled

---

## Phase 07: Security Configuration

**Time Estimate:** 15-20 minutes  
**Difficulty:** ⭐⭐ Intermediate

### SSH Server
- [ ] SSH installed
- [ ] SSH configured (port 1991, root disabled)
- [ ] SSH service enabled
- [ ] SSH tested (after first boot)

### UFW Firewall
- [ ] UFW installed
- [ ] UFW configured (outgoing allowed, SSH incoming)
- [ ] UFW enabled
- [ ] UFW rules verified

### Fail2ban
- [ ] Fail2ban installed
- [ ] Fail2ban configured for SSH
- [ ] Fail2ban service enabled

**Verification:**
```bash
systemctl is-active sshd
ss -tlnp | grep :1991
ufw status
fail2ban-client status sshd
```
- [ ] All security services active

---

## Phase 08: Hardware Configuration (Laptops)

**Time Estimate:** 10-20 minutes  
**Difficulty:** ⭐⭐ Intermediate

### Touchpad
- [ ] Touchpad configured
- [ ] Touchpad working correctly

### Webcam
- [ ] Webcam detected
- [ ] Webcam working

### IR Camera (If Present)
- [ ] IR camera detected
- [ ] Howdy installed and configured
- [ ] Face recognition working

### Fingerprint Reader (If Present)
- [ ] Fingerprint reader detected
- [ ] Fingerprint reader configured
- [ ] Fingerprints enrolled

**Verification:**
```bash
libinput list-devices | grep -i touchpad
lsusb | grep -i camera
lsusb | grep -i fingerprint
```
- [ ] Hardware detected and working

---

## Phase 09: Finalize

**Time Estimate:** 5 minutes  
**Difficulty:** ⭐ Easy

### Exit and Reboot
- [ ] Exited chroot environment
- [ ] All partitions unmounted
- [ ] Encrypted volumes closed (if used)
- [ ] System ready to reboot
- [ ] Rebooted into installed system

**Verification:**
- [ ] GRUB menu appears
- [ ] Can select Arch Linux
- [ ] LUKS passphrase prompt works (if encrypted)
- [ ] System boots successfully
- [ ] Can login with created user

---

## Post-Installation (After First Boot)

**See:** [Post-Installation Checklist](post-install-checklist.md)

### Essential Setup
- [ ] Network working (Ethernet/WiFi)
- [ ] System updates installed (`sudo pacman -Syu`)
- [ ] GPU drivers installed (if needed):
  - [ ] NVIDIA drivers (if NVIDIA GPU)
  - [ ] AMD drivers (if AMD GPU)
- [ ] Display server configured:
  - [ ] Xorg (if using Xorg)
  - [ ] Wayland (if using Wayland)

### Desktop Environment
- [ ] Desktop environment installed:
  - [ ] GNOME
  - [ ] KDE Plasma
  - [ ] XFCE
  - [ ] i3 Window Manager
  - [ ] Hyprland
- [ ] Desktop environment working
- [ ] Can login to desktop

### Applications
- [ ] Essential applications installed
- [ ] Web browser working
- [ ] Terminal emulator working
- [ ] File manager working

### Optional Enhancements
- [ ] Timeshift configured (backup)
- [ ] Additional applications installed
- [ ] System customized to preferences

---

## Installation Complete! 🎉

**Next Steps:**
- Review [Post-Installation Checklist](post-install-checklist.md)
- Check [Troubleshooting Guide](troubleshooting.md) if issues
- Explore [Comparison Guide](comparison-guide.md) for optimization
- Read [Performance Tuning](performance-tuning.md) for optimization

---

**Back to:** [Main Index](index.md) | [Repository Root](README.md)

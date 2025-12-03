# Quick Reference Guide

**Purpose:** Quick lookup for common installation tasks and module selection.

---

## 🚀 Quick Start Paths

### Path 1: Minimal Installation
```
core-installation.md → chroot.md → locale.md → user-creation.md → grub.md → networkmanager.md → exit-chroot.md
```

### Path 2: Encrypted Single Boot
```
disk-partitioning.md → luks-encryption.md → btrfs-filesystem.md → core-installation.md → chroot.md → locale.md → user-creation.md → grub.md → luks-keyfile-auto-unlock.md → exit-chroot.md
```

### Path 3: Dual Boot
```
PRE-install-windows.md → disk-partitioning.md → btrfs-filesystem.md → core-installation.md → chroot.md → locale.md → user-creation.md → grub.md → exit-chroot.md
```

---

## 📋 Module Selection Cheat Sheet

### Always Required
- ✅ `core-installation.md` - Base system
- ✅ `chroot.md` - Enter chroot
- ✅ `exit-chroot.md` - Final step

### Highly Recommended
- ⭐ `locale.md` - Set language/timezone
- ⭐ `user-creation.md` - Create user
- ⭐ `grub.md` - Bootloader
- ⭐ `networkmanager.md` - Network

### Disk Setup (Choose as needed)
- 💾 `disk-partitioning.md` - If disk not partitioned
- 🔒 `luks-encryption.md` - If want encryption
- 📁 `btrfs-filesystem.md` - If want Btrfs
- 📁 `mount-partitions.md` - If using ext4/xfs

### Network (Choose as needed)
- 📡 `networkmanager.md` - Always (for network)
- 📶 `wifi.md` - If using WiFi
- 📱 `bluetooth.md` - If using Bluetooth

### Security (Recommended)
- 🔐 `ssh-server.md` - If need remote access
- 🛡️ `ufw-firewall.md` - Recommended
- 🚫 `fail2ban.md` - If using SSH

### Hardware (Laptops)
- 🖱️ `touchpad.md` - Laptops
- 📷 `webcam.md` - Laptops
- 👁️ `ir-camera.md` - Laptops with IR
- 👆 `fingerprint.md` - Laptops with fingerprint

### Desktop (Choose one)
- 🖥️ `gnome.md` - GNOME desktop
- 🎨 `kde-plasma.md` - KDE Plasma
- 🪟 `xfce.md` - XFCE desktop
- ⚡ `i3wm.md` - i3 tiling WM
- 🌊 `hyprland.md` - Hyprland Wayland

### Display Server (Choose one)
- 🖼️ `xorg-config.md` - For Xorg-based DEs
- 🌐 `wayland-config.md` - For Wayland-based DEs

### Applications & Tools
- 📦 `essential-applications.md` - Basic apps
- 💾 `timeshift.md` - System backup
- 🌐 `w3m.md` - Terminal browser

### GPU Drivers (If needed)
- 🎮 `nvidia-drivers.md` - NVIDIA GPU
- 🎮 `amd-drivers.md` - AMD GPU

---

## 🔗 Common Command Reference

### Check System Status
```bash
# Check mounted partitions
mount | grep /mnt

# Check network
ping archlinux.org

# Check disk layout
lsblk

# Check service status
systemctl status <service>
```

### Essential Commands
```bash
# Install packages
pacman -S <package>

# Enable service
systemctl enable <service>

# Enter chroot
arch-chroot /mnt

# Generate fstab
genfstab -U /mnt >> /mnt/etc/fstab
```

---

## 📍 Module Locations

**Modules:** `modules/` - Detailed guides  
**Steps:** `steps/` - Commands only  
**Phases:** `phases/` - Organized groups  
**Wiki:** `wiki/` - Documentation

---

## 🎯 Use Case Finder

### "I want to..."
- **Install Arch Linux** → Start with `core-installation.md`
- **Encrypt my disk** → Use `luks-encryption.md`
- **Dual boot with Windows** → Use `PRE-install-windows.md` first
- **Set up WiFi** → Use `wifi.md` after `networkmanager.md`
- **Install a desktop** → Choose from `gnome.md`, `kde-plasma.md`, `xfce.md`
- **Configure touchpad** → Use `touchpad.md` (laptops)
- **Back up my system** → Use `timeshift.md`
- **Install GPU drivers** → Use `nvidia-drivers.md` or `amd-drivers.md`

---

## ⚡ Quick Links

- [Module Index](module-index.md) - All modules
- [Step Index](step-index.md) - All steps
- [Module Dependencies](module-dependencies.md) - Dependency graph
- [Installation Scenarios](installation-scenarios.md) - Pre-built scenarios
- [Generate Procedure](generate-procedure.md) - Build custom procedure

---

**Back to:** [Repository Root](README.md)

# Modules Directory

**Purpose:** This directory contains detailed installation and configuration modules. Each module provides comprehensive instructions, explanations, prerequisites, and troubleshooting for a specific installation or configuration task.

---

## What are Modules?

Modules are **detailed guides** that include:
- ✅ Complete step-by-step instructions
- ✅ Prerequisites and dependencies
- ✅ Explanations of what each step does
- ✅ Troubleshooting sections
- ✅ Links to official ArchWiki resources
- ✅ Context and background information

---

## Modules vs Steps

**Modules** (this directory):
- Detailed explanations
- Troubleshooting included
- Prerequisites documented
- Educational content

**Steps** (`../steps/` directory):
- Commands only (minimal text)
- Quick reference
- For experienced users
- Lightweight version

---

## Module Categories

### Pre-Installation Modules
- `PRE-create-arch-usb.md` - Create bootable USB drive
- `PRE-install-windows.md` - Install Windows (dual boot)
- `PRE-format-disk.md` - Format disk (fresh install)

### Core Installation
- `core-installation.md` - Install base Arch Linux system
- `chroot.md` - Enter chroot environment

### System Configuration
- `locale.md` - Set locale and timezone
- `user-creation.md` - Create user account
- `grub.md` - Install GRUB bootloader

### Disk & Filesystem
- `disk-partitioning.md` - Create partitions
- `luks-encryption.md` - Encrypt partitions
- `btrfs-filesystem.md` - Create Btrfs filesystem
- `mount-partitions.md` - Mount partitions

### Network & Audio
- `networkmanager.md` - Enable NetworkManager
- `wifi.md` - WiFi support
- `bluetooth.md` - Bluetooth support
- `audio.md` - PipeWire audio server

### Security
- `ssh-server.md` - SSH server configuration
- `ufw-firewall.md` - UFW firewall
- `fail2ban.md` - Fail2ban protection

### Hardware
- `touchpad.md` - Touchpad configuration
- `webcam.md` - Webcam configuration
- `ir-camera.md` - IR camera (face recognition)
- `fingerprint.md` - Fingerprint reader

### Desktop Environments & Window Managers
- `gnome.md` - GNOME desktop
- `kde-plasma.md` - KDE Plasma desktop
- `xfce.md` - XFCE desktop
- `i3wm.md` - i3 tiling window manager
- `hyprland.md` - Hyprland Wayland compositor

### Display Servers
- `xorg-config.md` - Xorg configuration
- `wayland-config.md` - Wayland configuration

### Applications & Tools
- `essential-applications.md` - Essential applications
- `timeshift.md` - System backup solution
- `w3m.md` - Terminal web browser

### GPU Drivers
- `nvidia-drivers.md` - NVIDIA drivers
- `amd-drivers.md` - AMD drivers

---

## How to Use Modules

1. **Start with prerequisites:** Check what's needed before running a module
2. **Follow in order:** Some modules depend on others
3. **Read explanations:** Understand what each command does
4. **Check troubleshooting:** If something goes wrong, check the troubleshooting section
5. **Refer to ArchWiki:** Modules link to official ArchWiki pages for more details

---

## Module Structure

Each module typically includes:
- **Purpose** - What this module does
- **Prerequisites** - What must be done first
- **Time** - Estimated time to complete
- **Environment** - Where to run (chroot, after boot, etc.)
- **Step-by-step instructions**
- **Troubleshooting** - Common issues and solutions
- **Official Resources** - Links to ArchWiki

---

## Quick Reference

- **Module Index:** See `../MODULE_index.md` for complete list
- **Generate Procedure:** See `../generate-procedure.md` to build custom installation
- **Installation Scenarios:** See `../installation-scenarios.md` for pre-built scenarios
- **Phases:** See `../phases/` for organized installation phases

---

**Back to:** [Repository Root](../README.md)

# Arch Linux Modular Installation System

**Welcome to the Arch Linux Modular Installation System**

This repository provides a **modular installation system** for [Arch Linux](https://wiki.archlinux.org/title/Arch_Linux), a lightweight and flexible Linux® distribution. Each component is a separate module that can be used independently or combined as needed.

---

## 🚀 Quick Start

1. **[Create Arch Linux USB](modules/PRE-01-create-arch-usb.md)** - Create bootable USB drive
2. **[Install Windows](modules/PRE-02-install-windows.md)** - Only if dual booting
3. **Boot from Arch Linux Live USB**
4. **Connect to network** (Ethernet or WiFi)
5. **Choose your installation path** below

---

## 📋 Installation Menu

### Pre-Installation Steps

Choose what you need before installing the system:

- **[Disk Partitioning](PRE-INSTALLATION.md#disk-partitioning)** - Create disk partitions (only if needed)
- **[LUKS Encryption](PRE-INSTALLATION.md#luks-encryption)** - Encrypt partitions
- **[Btrfs Filesystem](PRE-INSTALLATION.md#btrfs-filesystem)** - Create Btrfs with subvolumes
- **[Mount Partitions](PRE-INSTALLATION.md#mount-partitions)** - Mount partitions (non-Btrfs)

### Core Installation

**Required steps - follow in order:**

1. **[Core System Installation](CORE-INSTALLATION.md)** - Install base Arch Linux system (pacstrap)
2. **[Enter Chroot](CORE-INSTALLATION.md#enter-chroot)** - Enter chroot environment

### Post-Installation Configuration

Choose what you need after core installation:

- **[Locale & Timezone](POST-INSTALLATION.md#locale-timezone)** - Set locale and timezone
- **[User Creation](POST-INSTALLATION.md#user-creation)** - Create user account with sudo
- **[GRUB Bootloader](POST-INSTALLATION.md#grub-bootloader)** - Install GRUB
- **[LUKS Auto-Unlock](POST-INSTALLATION.md#luks-auto-unlock)** - Automatic LUKS decryption
- **[NetworkManager](POST-INSTALLATION.md#networkmanager)** - Enable NetworkManager
- **[WiFi Support](POST-INSTALLATION.md#wifi-support)** - WiFi configuration
- **[Bluetooth](POST-INSTALLATION.md#bluetooth)** - Bluetooth support
- **[Audio Server](POST-INSTALLATION.md#audio-server)** - PipeWire audio
- **[Exit & Reboot](POST-INSTALLATION.md#exit-reboot)** - Final step

---

## 📚 Complete Installation Guides

### Complete Installation (All Steps)

- **[Complete Installation Guide](COMPLETE-INSTALLATION.md)** - Full installation with all steps in one document

### Installation Flows

- **[Installation Flows](INSTALLATION-FLOWS.md)** - Pre-built installation scenarios
  - Minimal Installation - Just system + root password
  - Standard Installation - System + user + network
  - Full Installation - LUKS + Btrfs + all features

---

## 📖 Documentation

- **[Installation Scenarios](../INSTALLATION-SCENARIOS.md)** - Choose your installation scenario (dual boot, single boot, with/without LUKS)
- **[Phases](../phases/PREPARATION.md)** - **NEW!** Organized installation phases (recommended)
- **[Steps](../STEP_INDEX.md)** - **NEW!** Individual installation steps (commands only)
- **[Step Index](../STEP_INDEX.md)** - Quick reference for all steps
- **[Generate Procedure](../GENERATE_PROCEDURE.md)** - How to generate custom procedure
- **[Repository README](../README.md)** - System overview

---

## 🎯 Philosophy

**Modularity First:** Each installation step is a separate module. Pick and choose what you need.

**Complete Installation System:** This system covers the entire installation process from USB creation to system configuration, including Windows installation for dual boot scenarios.

**Flexibility:** Combine modules in any order to create your custom installation procedure.

---

**Start with:** [Pre-Installation Steps](PRE-INSTALLATION.md) or [Core Installation](CORE-INSTALLATION.md)

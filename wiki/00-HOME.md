# Arch Linux Modular Installation System - Intel Hardware

**Welcome to the Arch Linux Modular Installation System - Intel Hardware**

This repository provides a **modular installation system** for Arch Linux. Each component is a separate module that can be used independently or combined as needed.

---

## 🚀 Quick Start

1. **Boot from Arch Linux Live USB**
2. **Connect to network** (Ethernet or WiFi)
3. **Choose your installation path** below

---

## 📋 Installation Menu

### Pre-Installation Steps

Choose what you need before installing the system:

- **[Disk Partitioning](01-PRE-INSTALLATION.md#disk-partitioning)** - Create disk partitions (only if needed)
- **[LUKS Encryption](01-PRE-INSTALLATION.md#luks-encryption)** - Encrypt partitions
- **[Btrfs Filesystem](01-PRE-INSTALLATION.md#btrfs-filesystem)** - Create Btrfs with subvolumes
- **[Mount Partitions](01-PRE-INSTALLATION.md#mount-partitions)** - Mount partitions (non-Btrfs)

### Core Installation - Intel Hardware

**Required steps - follow in order:**

1. **[Core System Installation](02-CORE-INSTALLATION.md)** - Install base Arch Linux system (pacstrap)
2. **[Enter Chroot](02-CORE-INSTALLATION.md#enter-chroot)** - Enter chroot environment
3. **[Set Root Password](02-CORE-INSTALLATION.md#set-root-password)** - Set root password

### Post-Installation Configuration - Intel Hardware

Choose what you need after core installation:

- **[Locale & Timezone](03-POST-INSTALLATION.md#locale-timezone)** - Set locale and timezone
- **[User Creation](03-POST-INSTALLATION.md#user-creation)** - Create user account with sudo
- **[GRUB Bootloader](03-POST-INSTALLATION.md#grub-bootloader)** - Install GRUB
- **[LUKS Auto-Unlock](03-POST-INSTALLATION.md#luks-auto-unlock)** - Automatic LUKS decryption
- **[NetworkManager](03-POST-INSTALLATION.md#networkmanager)** - Enable NetworkManager
- **[WiFi Support](03-POST-INSTALLATION.md#wifi-support)** - WiFi configuration
- **[Bluetooth](03-POST-INSTALLATION.md#bluetooth)** - Bluetooth support
- **[Audio Server](03-POST-INSTALLATION.md#audio-server)** - PipeWire audio
- **[Exit & Reboot](03-POST-INSTALLATION.md#exit-reboot)** - Final step

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

- **[Module Index](../MODULE_INDEX.md)** - Quick reference for all modules
- **[Generate Procedure](../GENERATE_PROCEDURE.md)** - How to generate custom procedure
- **[Repository README](../README.md)** - System overview

---

## 🎯 Philosophy

**Modularity First:** Each installation step is a separate module. Pick and choose what you need.

**No Assumptions:** This system assumes you're already booted from Arch Linux Live USB. It doesn't format disks or create USB drives - it only installs and configures the system.

**Flexibility:** Combine modules in any order to create your custom installation procedure.

---

**Start with:** [Pre-Installation Steps](01-PRE-INSTALLATION.md) or [Core Installation](02-CORE-INSTALLATION.md)

# Arch Linux Modular Installation System

**Welcome to the Arch Linux Modular Installation System**

This repository provides a **modular installation system** for [Arch Linux](https://wiki.archlinux.org/title/Arch_Linux), a lightweight and flexible Linux® distribution. Each component is a separate module that can be used independently or combined as needed.

---

## 🚀 Quick Start

1. **[Create Arch Linux USB](../modules/pre-create-arch-usb.md)** - Create bootable USB drive
2. **[Install Windows](../modules/pre-install-windows.md)** - Only if dual booting
3. **Boot from Arch Linux Live USB**
4. **Connect to network** (Ethernet or WiFi)
5. **Choose your installation path** below

---

## 📋 Installation Menu

### Pre-Installation Steps

Choose what you need before installing the system:

- **[Disk Partitioning](pre-installation.md#disk-partitioning)** - Create disk partitions (only if needed)
- **[LUKS Encryption](pre-installation.md#luks-encryption)** - Encrypt partitions
- **[Btrfs Filesystem](pre-installation.md#btrfs-filesystem)** - Create Btrfs with subvolumes
- **[Mount Partitions](pre-installation.md#mount-partitions)** - Mount partitions (non-Btrfs)

### Core Installation

**Required steps - follow in order:**

1. **[Core System Installation](core-installation.md)** - Install base Arch Linux system (pacstrap)
2. **[Enter Chroot](core-installation.md#enter-chroot)** - Enter chroot environment

### Post-Installation Configuration

Choose what you need after core installation:

- **[Locale & Timezone](post-installation.md#locale-timezone)** - Set locale and timezone
- **[User Creation](post-installation.md#user-creation)** - Create user account with sudo
- **[GRUB Bootloader](post-installation.md#grub-bootloader)** - Install GRUB
- **[LUKS Auto-Unlock](post-installation.md#luks-auto-unlock)** - Automatic LUKS decryption
- **[NetworkManager](post-installation.md#networkmanager)** - Enable NetworkManager
- **[WiFi Support](post-installation.md#wifi-support)** - WiFi configuration
- **[Bluetooth](post-installation.md#bluetooth)** - Bluetooth support
- **[Audio Server](post-installation.md#audio-server)** - PipeWire audio
- **[Exit & Reboot](post-installation.md#exit-reboot)** - Final step

---

## 📚 Complete Installation Guides

### Complete Installation (All Steps)

- **[Complete Installation Guide](complete-installation.md)** - Full installation with all steps in one document

### Installation Flows

- **[Installation Flows](installation-flows.md)** - Pre-built installation scenarios
  - Minimal Installation - Just system + root password
  - Standard Installation - System + user + network
  - Full Installation - LUKS + Btrfs + all features

---

## 📖 Documentation

- **[Installation Scenarios](../installation-scenarios.md)** - Choose your installation scenario (dual boot, single boot, with/without LUKS)
- **[Phases](../phases/00-preparation.md)** - **NEW!** Organized installation phases (recommended)
- **[Steps](../step-index.md)** - **NEW!** Individual installation steps (commands only)
- **[Step Index](../step-index.md)** - Quick reference for all steps
- **[Generate Procedure](../generate-procedure.md)** - How to generate custom procedure
- **[Repository README](../README.md)** - System overview

---

## 🎯 Philosophy

**Modularity First:** Each installation step is a separate module. Pick and choose what you need.

**Complete Installation System:** This system covers the entire installation process from USB creation to system configuration, including Windows installation for dual boot scenarios.

**Flexibility:** Combine modules in any order to create your custom installation procedure.

---

**Start with:** [Pre-Installation Steps](pre-installation.md) or [Core Installation](core-installation.md)

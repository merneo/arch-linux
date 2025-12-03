# Complete Arch Linux Installation Guide

**Purpose:** This guide provides a complete, sequential path through the modular Arch Linux installation process by linking to the relevant, detailed documentation modules. For a comprehensive overview of the Arch Linux installation process, refer to the [ArchWiki Installation Guide](https://wiki.archlinux.org/title/Installation_guide).

---

## 🚀 Quick Start (Recommended Flow)

Follow these phases in order for a comprehensive Arch Linux installation. Each link will take you to a detailed module covering that step.

1.  **[Preparation](phases/00-PREPARATION.md)**
    *   Create Arch Linux USB
    *   Install Windows (if dual-booting)
    *   Format Disk (if starting fresh)

2.  **[Disk Setup](phases/01-DISK_SETUP.md)**
    *   Partitioning
    *   LUKS Encryption (optional)
    *   Btrfs Filesystem (optional)
    *   Mount Partitions

3.  **[System Install](phases/02-SYSTEM_INSTALL.md)**
    *   Core Installation (pacstrap)
    *   Generate Fstab

4.  **[Basic Configuration](phases/03-BASIC_CONFIG.md)**
    *   Set Locale & Timezone
    *   Create User Account
    *   Configure Sudo Access

5.  **[Bootloader](phases/04-BOOTLOADER.md)**
    *   Install GRUB
    *   Configure GRUB for LUKS Auto-Unlock (if encrypted)

6.  **[Network](phases/05-NETWORK.md)**
    *   Enable NetworkManager
    *   Configure WiFi Support
    *   Configure Bluetooth Support

7.  **[Audio](phases/06-AUDIO.md)**
    *   Install PipeWire Audio Stack

8.  **[Security](phases/07-SECURITY.md)**
    *   Configure UFW Firewall
    *   Install Fail2ban

9.  **[Laptop Hardware (Optional)](phases/08-LAPTOP_HARDWARE.md)**
    *   Touchpad
    *   Webcam
    *   IR Camera (for face recognition)
    *   Fingerprint Reader

10. **[Finalize Installation](phases/09-FINALIZE.md)**
    *   Exit Chroot
    *   Unmount Partitions
    *   Reboot System

---

**Tip:** For individual commands or more detailed explanations, refer to the [Module Index](MODULE_INDEX.md) or [Step Index](STEP_INDEX.md)
# Arch Linux Modular Installation System

**Repository:** https://github.com/merneo/arch-linux

This repository provides a **modular installation system** for Arch Linux. This approach allows users to build a customized installation procedure by selecting and combining independent modules. For a comprehensive overview of the Arch Linux installation process, refer to the [ArchWiki Installation Guide](https://wiki.archlinux.org/title/Installation_guide).

---

## 🚀 Start Here

**👉 [Main Index](index.md) - Choose your hardware and configuration**

The main index provides links to all installation scenarios organized by:
- Hardware type (Desktop/Laptop, Intel/AMD, NVIDIA/AMD GPU)
- Installation type (Single boot/Dual boot, Encrypted/Non-encrypted)
- Desktop environment preferences
- Complete phase-by-phase navigation

**Quick Links:**
- **[Installation Phases](phases/00-preparation.md)** - Follow phases in order (recommended)
- **[Installation Scenarios](installation-scenarios.md)** - Pre-built scenarios
- **[Complete Guide](wiki/complete-installation.md)** - All steps in one document

---

## Philosophy

**Modularity First:** Each installation step is a separate module. Pick and choose what you need.

**Complete Installation System:** This system covers the entire installation process from USB creation to system configuration, including Windows installation for dual boot scenarios.

**Core is Minimal:** Core installation = pacstrap + initial configuration. Nothing else.

**Flexibility:** Combine modules in any order to create your custom installation procedure.

---

## Repository Structure

```
arch-linux/
├── README.md                    # This file
├── module-index.md             # Quick reference for all modules
├── generate-procedure.md       # How to generate custom procedure
├── phases/                     # Installation phases (10 phases)
│   ├── 00-preparation.md         # Phase 0: USB, Windows, disk formatting
│   ├── 01-disk-setup.md          # Phase 1: Partitioning, encryption, filesystem
│   ├── 02-system-install.md      # Phase 2: Base system, chroot
│   ├── 03-basic-config.md        # Phase 3: Locale, user, root password
│   ├── 04-bootloader.md          # Phase 4: GRUB, LUKS auto-unlock
│   ├── 05-network.md             # Phase 5: NetworkManager, WiFi, Bluetooth
│   ├── 06-audio.md               # Phase 6: PipeWire audio
│   ├── 07-security.md            # Phase 7: SSH, UFW, fail2ban
│   ├── 08-hardware.md            # Phase 8: Touchpad, webcam, IR, fingerprint
│   └── 09-finalize.md            # Phase 9: Exit chroot, reboot
├── steps/                      # Individual steps (commands only)
│   ├── audio-commands.md
│   ├── bluetooth-commands.md
│   ├── btrfs-filesystem-commands.md
│   ├── chroot-commands.md
│   ├── core-installation-commands.md
│   ├── disk-partitioning-commands.md
│   ├── exit-chroot-commands.md
│   ├── fail2ban-commands.md
│   ├── fingerprint-commands.md
│   ├── grub-commands.md
│   ├── ir-camera-commands.md
│   ├── locale-commands.md
│   ├── luks-encryption-commands.md
│   ├── luks-keyfile-auto-unlock-commands.md
│   ├── mount-partitions-commands.md
│   ├── networkmanager-commands.md
│   ├── pre-create-arch-usb-commands.md
│   ├── pre-format-disk-commands.md
│   ├── pre-install-windows-commands.md
│   ├── README.md
│   ├── ssh-server-commands.md
│   ├── touchpad-commands.md
│   ├── ufw-firewall-commands.md
│   ├── user-creation-commands.md
│   ├── webcam-commands.md
│   └── wifi-commands.md
├── modules/                    # Detailed modules (34 modules)
│   ├── pre-create-arch-usb.md
│   ├── pre-install-windows.md
│   ├── pre-format-disk.md
│   ├── core-installation.md
│   ├── chroot.md
│   ├── locale.md
│   ├── user-creation.md
│   ├── grub.md
│   ├── disk-partitioning.md
│   ├── luks-encryption.md
│   ├── btrfs-filesystem.md
│   ├── mount-partitions.md
│   ├── luks-keyfile-auto-unlock.md
│   ├── networkmanager.md
│   ├── wifi.md
│   ├── bluetooth.md
│   ├── audio.md
│   ├── exit-chroot.md
│   ├── touchpad.md
│   ├── webcam.md
│   ├── ir-camera.md
│   ├── fingerprint.md
│   ├── ssh-server.md
│   ├── ufw-firewall.md
│   ├── fail2ban.md
│   ├── gnome.md
│   ├── kde-plasma.md
│   ├── xfce.md
│   ├── i3wm.md
│   ├── xorg-config.md
│   ├── wayland-config.md
│   ├── essential-applications.md
│   ├── timeshift.md
│   ├── w3m.md
│   ├── nvidia-drivers.md
│   └── amd-drivers.md
└── wiki/                       # Wiki documentation
    ├── complete-installation.md
    ├── core-installation.md
    ├── HOME.md                 # Home page with menu
    ├── installation-flows.md
    ├── post-installation.md
    ├── pre-installation.md
    └── README.md

```

---

## Quick Reference

### Getting Started
- **[Main Index](index.md)** - Start here! Choose your hardware and configuration
- **[Installation Checklist](installation-checklist.md)** - Track your installation progress
- **[Installation Scenarios](installation-scenarios.md)** - Choose installation scenario (dual boot, single boot, with/without LUKS)
- **[Time Estimates](time-estimates.md)** - Plan your installation time

### Installation Guides
- **[Phases](phases/README.md)** - Organized installation phases (recommended approach)
- **[Step Index](step-index.md)** - Quick reference for all steps
- **[Module Index](module-index.md)** - Quick reference for all modules
- **[Post-Installation Checklist](post-install-checklist.md)** - What to do after first boot

### Help & Support
- **[FAQ](faq.md)** - Frequently asked questions
- **[Troubleshooting](troubleshooting.md)** - Common problems and solutions
- **[Common Mistakes](common-mistakes.md)** - Avoid common errors

### Reference & Tools
- **[Module Dependencies](module-dependencies.md)** - Dependency graph and relationships
- **[Module Finder](module-finder.md)** - Find modules by use case, hardware, or scenario
- **[Quick Reference](quick-reference.md)** - Cheat sheet and quick lookup
- **[Comparison Guide](comparison-guide.md)** - Xorg/Wayland, Desktop Environments comparison
- **[Desktop vs Laptop](desktop-vs-laptop.md)** - Guide to choose steps based on hardware type

### Optimization & Maintenance
- **[Performance Tuning](performance-tuning.md)** - Optimize your system performance
- **[Backup & Recovery](backup-recovery.md)** - Backup and recovery procedures

### Contributing
- **[Generate Procedure](generate-procedure.md)** - How to generate custom installation procedure
- **[Contributing Guide](contributing.md)** - How to contribute to this repository
- **[Changelog](changelog.md)** - Change history

## Example Installation Flows

These examples demonstrate how to combine modules for common installation types. For a detailed decision tree, refer to [Installation Flows](wiki/installation-flows.md).

### Desktop Computer (Minimal)
1. `modules/core-installation.md`
2. `modules/chroot.md`
3. `modules/grub.md`
4. `modules/networkmanager.md` (if using Ethernet)
5. `modules/exit-chroot.md`
6. **After first boot:**
   - `modules/xorg-config.md` or `modules/wayland-config.md` (display server)
   - `modules/essential-applications.md` (web browser, etc.)

### Desktop Computer (Full)
1. `modules/disk-partitioning.md` (if needed)
2. `modules/luks-encryption.md`
3. `modules/btrfs-filesystem.md`
4. `modules/core-installation.md`
5. `modules/chroot.md`
6. `modules/locale.md`
7. `modules/user-creation.md`
8. `modules/grub.md`
9. `modules/luks-keyfile-auto-unlock.md`
10. `modules/networkmanager.md`
11. `modules/audio.md`
12. `modules/exit-chroot.md`
13. **After first boot:**
    - `modules/xorg-config.md` or `modules/wayland-config.md`
    - `modules/gnome.md`, `modules/kde-plasma.md`, or `modules/xfce.md` (desktop environment)
    - `modules/essential-applications.md`
    - `modules/timeshift.md` (backup)
    - `modules/nvidia-drivers.md` or `modules/amd-drivers.md` (GPU drivers)

### Laptop (Minimal)
1. `modules/core-installation.md`
2. `modules/chroot.md`
3. `modules/grub.md`
4. `modules/networkmanager.md`
5. `modules/wifi.md`
6. `modules/exit-chroot.md`
7. **After first boot:**
   - `modules/touchpad.md`
   - `modules/webcam.md`
   - `modules/xorg-config.md` or `modules/wayland-config.md` (display server)
   - `modules/essential-applications.md` (web browser, etc.)

### Laptop (Full with Biometrics)
1. `modules/disk-partitioning.md` (if needed)
2. `modules/luks-encryption.md`
3. `modules/btrfs-filesystem.md`
4. `modules/core-installation.md`
5. `modules/chroot.md`
6. `modules/locale.md`
7. `modules/user-creation.md`
8. `modules/grub.md`
9. `modules/luks-keyfile-auto-unlock.md`
10. `modules/networkmanager.md`
11. `modules/wifi.md`
12. `modules/bluetooth.md`
13. `modules/audio.md`
14. `modules/exit-chroot.md`
15. **After first boot:**
    - `modules/touchpad.md`
    - `modules/webcam.md`
    - `modules/ir-camera.md` (if IR camera present)
    - `modules/fingerprint.md` (if fingerprint reader present)
    - `modules/xorg-config.md` or `modules/wayland-config.md`
    - `modules/gnome.md`, `modules/kde-plasma.md`, or `modules/xfce.md`
    - `modules/essential-applications.md`
    - `modules/timeshift.md`
    - `modules/nvidia-drivers.md` or `modules/amd-drivers.md`

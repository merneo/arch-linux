# Arch Linux Modular Installation System

**Repository:** https://github.com/merneo/arch-linux

This repository provides a **modular installation system** for Arch Linux. This approach allows users to build a customized installation procedure by selecting and combining independent modules. For a comprehensive overview of the Arch Linux installation process, refer to the [ArchWiki Installation Guide](https://wiki.archlinux.org/title/Installation_guide).

---

## 🚀 Quick Start

**Start here:** [Installation Scenarios](INSTALLATION-SCENARIOS.md) - Choose your scenario

**Or browse:**
- [Phases](phases/) - Organized installation phases (recommended)
- [Steps](steps/) - Individual installation steps
- [Home](wiki/00-HOME.md) - Installation menu
- [Complete Guide](wiki/COMPLETE-INSTALLATION.md) - All steps in one document

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
├── MODULE_INDEX.md             # Quick reference for all modules
├── GENERATE_PROCEDURE.md       # How to generate custom procedure
├── phases/                     # Installation phases (10 phases)
│   ├── 00-PREPARATION.md       # Phase 0: USB, Windows, disk formatting
│   ├── 01-DISK_SETUP.md        # Phase 1: Partitioning, encryption, filesystem
│   ├── 02-SYSTEM_INSTALL.md    # Phase 2: Base system, chroot
│   ├── 03-BASIC_CONFIG.md      # Phase 3: Locale, user, root password
│   ├── 04-BOOTLOADER.md        # Phase 4: GRUB, LUKS auto-unlock
│   ├── 05-NETWORK.md           # Phase 5: NetworkManager, WiFi, Bluetooth
│   ├── 06-AUDIO.md             # Phase 6: PipeWire audio
│   ├── 07-SECURITY.md          # Phase 7: SSH, UFW, fail2ban
│   ├── 08-LAPTOP_HARDWARE.md   # Phase 8: Touchpad, webcam, IR, fingerprint
│   └── 09-FINALIZE.md          # Phase 9: Exit chroot, reboot
├── steps/                      # Individual steps (25 steps: commands only)
│   ├── PRE-01-create-arch-usb.md
│   ├── PRE-02-install-windows.md
│   ├── PRE-03-format-disk.md
│   ├── 00-core-installation.md
│   ├── 01-chroot.md
│   ├── 02-locale.md
│   ├── 04-user-creation.md
│   ├── 05-grub.md
│   ├── 06-disk-partitioning.md
│   ├── 07-luks-encryption.md
│   ├── 08-btrfs-filesystem.md
│   ├── 09-mount-partitions.md
│   ├── 10-luks-keyfile-auto-unlock.md
│   ├── 11-networkmanager.md
│   ├── 12-wifi.md
│   ├── 13-bluetooth.md
│   ├── 14-audio.md
│   ├── 15-exit-chroot.md
│   ├── 17-laptop-touchpad.md
│   ├── 18-laptop-webcam.md
│   ├── 19-laptop-ir-camera.md
│   ├── 20-laptop-fingerprint.md
│   ├── 21-ssh-server.md
│   ├── 22-ufw-firewall.md
│   └── 23-fail2ban.md
├── modules/                    # Detailed modules (33 modules)
│   ├── PRE-01-create-arch-usb.md
│   ├── PRE-02-install-windows.md
│   ├── PRE-03-format-disk.md
│   ├── 00-core-installation.md
│   ├── 01-chroot.md
│   ├── 02-locale.md
│   ├── 04-user-creation.md
│   ├── 05-grub.md
│   ├── 06-disk-partitioning.md
│   ├── 07-luks-encryption.md
│   ├── 08-btrfs-filesystem.md
│   ├── 09-mount-partitions.md
│   ├── 10-luks-keyfile-auto-unlock.md
│   ├── 11-networkmanager.md
│   ├── 12-wifi.md
│   ├── 13-bluetooth.md
│   ├── 14-audio.md
│   ├── 15-exit-chroot.md
│   ├── 17-laptop-touchpad.md
│   ├── 18-laptop-webcam.md
│   ├── 19-laptop-ir-camera.md
│   ├── 20-laptop-fingerprint.md
│   ├── 21-ssh-server.md
│   ├── 22-ufw-firewall.md
│   ├── 23-fail2ban.md
│   ├── 24-gnome.md
│   ├── 25-kde-plasma.md
│   ├── 26-xfce.md
│   ├── 27-xorg-config.md
│   ├── 28-wayland-config.md
│   ├── 29-essential-applications.md
│   ├── 30-timeshift.md
│   ├── 31-nvidia-drivers.md
│   └── 32-amd-drivers.md
└── wiki/                       # Wiki documentation
    ├── 00-HOME.md              # Home page with menu
    ├── 01-PRE-INSTALLATION.md  # Pre-installation steps
    ├── 02-CORE-INSTALLATION.md # Core installation (DEFAULT VIEW)
    ├── 03-POST-INSTALLATION.md # Post-installation configuration
    ├── COMPLETE-INSTALLATION.md # Complete guide (all steps)
    └── INSTALLATION-FLOWS.md   # Pre-built installation scenarios

```

---

## Quick Reference

- **[Installation Scenarios](INSTALLATION-SCENARIOS.md)** - Choose installation scenario (dual boot, single boot, with/without LUKS)
- **[Phases](phases/)** - Organized installation phases (recommended approach)
- **[Step Index](STEP_INDEX.md)** - Quick reference for all steps
- **[Desktop vs Laptop](DESKTOP_VS_LAPTOP.md)** - Guide to choose steps based on hardware type
- **[Generate Procedure](GENERATE_PROCEDURE.md)** - How to generate custom installation procedure

## Example Installation Flows

These examples demonstrate how to combine modules for common installation types. For a detailed decision tree, refer to [Installation Flows](INSTALLATION-FLOWS.md).

### Desktop Computer (Minimal)
1. `00-core-installation.md`
2. `01-chroot.md`
3. `05-grub.md`
4. `11-networkmanager.md` (if using Ethernet)
5. `15-exit-chroot.md`
6. **After first boot:**
   - `27-xorg-config.md` or `28-wayland-config.md` (display server)
   - `29-essential-applications.md` (web browser, etc.)

### Desktop Computer (Full)
1. `06-disk-partitioning.md` (if needed)
2. `07-luks-encryption.md`
3. `08-btrfs-filesystem.md`
4. `00-core-installation.md`
5. `01-chroot.md`
6. `02-locale.md`
7. `04-user-creation.md`
8. `05-grub.md`
9. `10-luks-keyfile-auto-unlock.md`
10. `11-networkmanager.md`
11. `14-audio.md`
12. `15-exit-chroot.md`
13. **After first boot:**
    - `27-xorg-config.md` or `28-wayland-config.md`
    - `24-gnome.md`, `25-kde-plasma.md`, or `26-xfce.md` (desktop environment)
    - `29-essential-applications.md`
    - `30-timeshift.md` (backup)
    - `31-nvidia-drivers.md` or `32-amd-drivers.md` (GPU drivers)

### Laptop (Minimal)
1. `00-core-installation.md`
2. `01-chroot.md`
3. `05-grub.md`
4. `11-networkmanager.md`
5. `12-wifi.md`
6. `15-exit-chroot.md`
7. **After first boot:**
   - `17-laptop-touchpad.md`
   - `18-laptop-webcam.md`
   - `27-xorg-config.md` or `28-wayland-config.md` (display server)
   - `29-essential-applications.md` (web browser, etc.)

### Laptop (Full with Biometrics)
1. `06-disk-partitioning.md` (if needed)
2. `07-luks-encryption.md`
3. `08-btrfs-filesystem.md`
4. `00-core-installation`
5. `01-chroot.md`
6. `02-locale.md`
7. `04-user-creation.md`
8. `05-grub.md`
9. `10-luks-keyfile-auto-unlock.md`
10. `11-networkmanager.md`
11. `12-wifi.md`
12. `13-bluetooth.md`
13. `14-audio.md`
14. `15-exit-chroot.md`
15. **After first boot:**
    - `17-laptop-touchpad.md`
    - `18-laptop-webcam.md`
    - `19-laptop-ir-camera.md` (if IR camera present)
    - `20-laptop-fingerprint.md` (if fingerprint reader present)
    - `27-xorg-config.md` or `28-wayland-config.md`
    - `24-gnome.md`, `25-kde-plasma.md`, or `26-xfce.md`
    - `29-essential-applications.md`
    - `30-timeshift.md`
    - `31-nvidia-drivers.md` or `32-amd-drivers.md`

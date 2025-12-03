# Arch Linux Modular Installation System

**Repository:** https://github.com/merneo/arch-linux

This repository provides a **modular installation system** for Arch Linux. This approach allows users to build a customized installation procedure by selecting and combining independent modules. For a comprehensive overview of the Arch Linux installation process, refer to the [ArchWiki Installation Guide](https://wiki.archlinux.org/title/Installation_guide).

---

## 🚀 Start Here

**👉 [Main Index](INDEX.md) - Choose your hardware and configuration**

The main index provides links to all installation scenarios organized by:
- Hardware type (Desktop/Laptop, Intel/AMD, NVIDIA/AMD GPU)
- Installation type (Single boot/Dual boot, Encrypted/Non-encrypted)
- Desktop environment preferences
- Complete phase-by-phase navigation

**Quick Links:**
- **[Installation Phases](phases/PREPARATION.md)** - Follow phases in order (recommended)
- **[Installation Scenarios](INSTALLATION-SCENARIOS.md)** - Pre-built scenarios
- **[Complete Guide](wiki/COMPLETE-INSTALLATION.md)** - All steps in one document

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
│   ├── PREPARATION.md          # Phase 0: USB, Windows, disk formatting
│   ├── DISK_SETUP.md           # Phase 1: Partitioning, encryption, filesystem
│   ├── SYSTEM_INSTALL.md       # Phase 2: Base system, chroot
│   ├── BASIC_CONFIG.md         # Phase 3: Locale, user, root password
│   ├── BOOTLOADER.md           # Phase 4: GRUB, LUKS auto-unlock
│   ├── NETWORK.md              # Phase 5: NetworkManager, WiFi, Bluetooth
│   ├── AUDIO.md                # Phase 6: PipeWire audio
│   ├── SECURITY.md             # Phase 7: SSH, UFW, fail2ban
│   ├── HARDWARE.md      # Phase 8: Touchpad, webcam, IR, fingerprint
│   └── FINALIZE.md             # Phase 9: Exit chroot, reboot
├── steps/                      # Individual steps (25 steps: commands only)
│   ├── PRE-create-arch-usb.md
│   ├── PRE-install-windows.md
│   ├── PRE-format-disk.md
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
│   └── fail2ban.md
├── modules/                    # Detailed modules (34 modules)
│   ├── PRE-create-arch-usb.md
│   ├── PRE-install-windows.md
│   ├── PRE-format-disk.md
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
    ├── HOME.md                 # Home page with menu
    ├── PRE-INSTALLATION.md     # Pre-installation steps
    ├── CORE-INSTALLATION.md    # Core installation (DEFAULT VIEW)
    ├── POST-INSTALLATION.md    # Post-installation configuration
    ├── COMPLETE-INSTALLATION.md # Complete guide (all steps)
    └── INSTALLATION-FLOWS.md   # Pre-built installation scenarios

```

---

## Quick Reference

- **[Installation Scenarios](INSTALLATION-SCENARIOS.md)** - Choose installation scenario (dual boot, single boot, with/without LUKS)
- **[Phases](phases/)** - Organized installation phases (recommended approach)
- **[Step Index](STEP_INDEX.md)** - Quick reference for all steps
- **[Module Index](MODULE_INDEX.md)** - Quick reference for all modules
- **[Module Dependencies](MODULE_DEPENDENCIES.md)** - Dependency graph and relationships
- **[Module Finder](MODULE_FINDER.md)** - Find modules by use case, hardware, or scenario
- **[Quick Reference](QUICK_REFERENCE.md)** - Cheat sheet and quick lookup
- **[Desktop vs Laptop](DESKTOP_VS_LAPTOP.md)** - Guide to choose steps based on hardware type
- **[Generate Procedure](GENERATE_PROCEDURE.md)** - How to generate custom installation procedure

## Example Installation Flows

These examples demonstrate how to combine modules for common installation types. For a detailed decision tree, refer to [Installation Flows](wiki/INSTALLATION-FLOWS.md).

### Desktop Computer (Minimal)
1. `core-installation.md`
2. `chroot.md`
3. `grub.md`
4. `networkmanager.md` (if using Ethernet)
5. `exit-chroot.md`
6. **After first boot:**
   - `xorg-config.md` or `wayland-config.md` (display server)
   - `essential-applications.md` (web browser, etc.)

### Desktop Computer (Full)
1. `disk-partitioning.md` (if needed)
2. `luks-encryption.md`
3. `btrfs-filesystem.md`
4. `core-installation.md`
5. `chroot.md`
6. `locale.md`
7. `user-creation.md`
8. `grub.md`
9. `luks-keyfile-auto-unlock.md`
10. `networkmanager.md`
11. `audio.md`
12. `exit-chroot.md`
13. **After first boot:**
    - `xorg-config.md` or `wayland-config.md`
    - `gnome.md`, `kde-plasma.md`, or `xfce.md` (desktop environment)
    - `essential-applications.md`
    - `timeshift.md` (backup)
    - `nvidia-drivers.md` or `amd-drivers.md` (GPU drivers)

### Laptop (Minimal)
1. `core-installation.md`
2. `chroot.md`
3. `grub.md`
4. `networkmanager.md`
5. `wifi.md`
6. `exit-chroot.md`
7. **After first boot:**
   - `touchpad.md`
   - `webcam.md`
   - `xorg-config.md` or `wayland-config.md` (display server)
   - `essential-applications.md` (web browser, etc.)

### Laptop (Full with Biometrics)
1. `disk-partitioning.md` (if needed)
2. `luks-encryption.md`
3. `btrfs-filesystem.md`
4. `core-installation.md`
5. `chroot.md`
6. `locale.md`
7. `user-creation.md`
8. `grub.md`
9. `luks-keyfile-auto-unlock.md`
10. `networkmanager.md`
11. `wifi.md`
12. `bluetooth.md`
13. `audio.md`
14. `exit-chroot.md`
15. **After first boot:**
    - `touchpad.md`
    - `webcam.md`
    - `ir-camera.md` (if IR camera present)
    - `fingerprint.md` (if fingerprint reader present)
    - `xorg-config.md` or `wayland-config.md`
    - `gnome.md`, `kde-plasma.md`, or `xfce.md`
    - `essential-applications.md`
    - `timeshift.md`
    - `nvidia-drivers.md` or `amd-drivers.md`

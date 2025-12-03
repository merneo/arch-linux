# Arch Linux Modular Installation System

**Repository:** https://github.com/merneo/arch-linux

This repository provides a **modular installation system** for Arch Linux. Each component is a separate module that can be used independently or combined as needed.

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

**Core is Minimal:** Core installation = pacstrap + root password. Nothing else.

**Flexibility:** Combine modules in any order to create your custom installation procedure.

---

## Installation Structure

**New Structure: Phases → Steps → Commands**

### Installation Phases (Recommended)

**Follow phases for organized installation:**

1. **[Phase 00: Preparation](phases/00-PREPARATION.md)** - USB creation, Windows installation, disk formatting
2. **[Phase 01: Disk Setup](phases/01-DISK_SETUP.md)** - Partitioning, encryption, filesystem
3. **[Phase 02: System Install](phases/02-SYSTEM_INSTALL.md)** - Base system installation, chroot
4. **[Phase 03: Basic Config](phases/03-BASIC_CONFIG.md)** - Locale, root password, user creation
5. **[Phase 04: Bootloader](phases/04-BOOTLOADER.md)** - GRUB installation, LUKS auto-unlock
6. **[Phase 05: Network](phases/05-NETWORK.md)** - NetworkManager, WiFi, Bluetooth
7. **[Phase 06: Audio](phases/06-AUDIO.md)** - PipeWire audio
8. **[Phase 07: Security](phases/07-SECURITY.md)** - SSH, UFW, fail2ban
9. **[Phase 08: Laptop Hardware](phases/08-LAPTOP_HARDWARE.md)** - Touchpad, webcam, IR camera, fingerprint
10. **[Phase 09: Finalize](phases/09-FINALIZE.md)** - Exit chroot, reboot

### Individual Steps

**For custom procedures, use individual [steps](steps/):**

- **Preparation:** `PRE-01-create-arch-usb.md`, `PRE-02-install-windows.md`, `PRE-03-format-disk.md`
- **Core:** `00-core-installation.md`, `01-chroot.md`
- **Configuration:** `02-locale.md`, `03-root-password.md`, `04-user-creation.md`, `05-grub.md`
- **Disk:** `06-disk-partitioning.md`, `07-luks-encryption.md`, `08-btrfs-filesystem.md`, `09-mount-partitions.md`
- **Advanced:** `10-luks-keyfile-auto-unlock.md`, `11-networkmanager.md`, `12-wifi.md`, `13-bluetooth.md`, `14-audio.md`
- **Security:** `21-ssh-server.md`, `22-ufw-firewall.md`, `23-fail2ban.md`
- **Laptop:** `17-laptop-touchpad.md`, `18-laptop-webcam.md`, `19-laptop-ir-camera.md`, `20-laptop-fingerprint.md`
- **Final:** `15-exit-chroot.md`

---

## Quick Reference

- **[Installation Scenarios](INSTALLATION-SCENARIOS.md)** - Choose installation scenario (dual boot, single boot, with/without LUKS)
- **[Phases](phases/)** - Organized installation phases (recommended approach)
- **[Steps](steps/)** - Individual installation steps (for custom procedures)
- **[Step Index](STEP_INDEX.md)** - Quick reference for all steps
- **[Desktop vs Laptop](DESKTOP_VS_LAPTOP.md)** - Guide to choose steps based on hardware type
- **[Generate Procedure](GENERATE_PROCEDURE.md)** - How to generate custom installation procedure

## Example Installation Flows

### Minimal Installation (Just System + Root Password)

1. `00-core-installation.md` - Install base system
2. `01-chroot.md` - Enter chroot
3. `03-root-password.md` - Set root password
4. `05-grub.md` - Install GRUB
5. `15-exit-chroot.md` - Exit and reboot

**Note:** Assumes disk is already partitioned and mounted at `/mnt`

### Full Installation (LUKS + Btrfs + All Features)

1. `06-disk-partitioning.md` - Create partitions (if needed)
2. `07-luks-encryption.md` - Encrypt partitions
3. `08-btrfs-filesystem.md` - Create Btrfs with subvolumes
4. `00-core-installation.md` - Install base system
5. `01-chroot.md` - Enter chroot
6. `02-locale.md` - Set locale
7. `03-root-password.md` - Set root password
8. `04-user-creation.md` - Create user
9. `05-grub.md` - Install GRUB (with LUKS parameters)
10. `10-luks-keyfile-auto-unlock.md` - Auto-unlock (optional)
11. `11-networkmanager.md` - Enable NetworkManager
12. `12-wifi.md` - WiFi support
13. `13-bluetooth.md` - Bluetooth
14. `14-audio.md` - Audio server
15. `15-exit-chroot.md` - Exit and reboot

### Custom Flow

Pick modules you need and follow them in logical order. Each module lists its prerequisites.

---

## Module Structure

Each module follows this structure:

1. **Purpose** - What this module does
2. **Prerequisites** - Required modules or conditions
3. **Time** - Estimated time to complete
4. **Steps** - Detailed instructions
5. **Success** - Verification steps
6. **Next** - Suggested following modules

---

## Usage Tips

- **Read prerequisites** before starting each module
- **Verify success** after each module
- **Customize** commands (usernames, timezones, etc.)
- **Skip modules** you don't need
- **Combine** modules in any logical order

---

## Documentation

### Wiki Documentation (User-Friendly)

- **[Home](wiki/00-HOME.md)** - Installation menu
- **[Core Installation](wiki/02-CORE-INSTALLATION.md)** - **DEFAULT VIEW** - Core installation steps
- **[Pre-Installation](wiki/01-PRE-INSTALLATION.md)** - Disk partitioning, LUKS, Btrfs
- **[Post-Installation](wiki/03-POST-INSTALLATION.md)** - Configuration menu
- **[Complete Guide](wiki/COMPLETE-INSTALLATION.md)** - All steps in one document
- **[Installation Flows](wiki/INSTALLATION-FLOWS.md)** - Pre-built scenarios

### Step Documentation (Technical)

- **[Step Index](STEP_INDEX.md)** - Quick reference for all steps
- **[Phases](phases/)** - Organized installation phases
- **[Steps](steps/)** - Individual step files (commands only)
- **[Generate Procedure](GENERATE_PROCEDURE.md)** - How to generate custom procedure

### AI Helpers

- **Helper:** `~/Documents/helpers/arch-linux-modular-installation.md` - Quick reference for AI assistants

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
├── steps/                      # Individual steps (24 steps: commands only)
│   ├── PRE-01-create-arch-usb.md
│   ├── PRE-02-install-windows.md
│   ├── PRE-03-format-disk.md
│   ├── 00-core-installation.md
│   ├── 01-chroot.md
│   ├── 02-locale.md
│   ├── 03-root-password.md
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
├── modules/                    # Legacy modules (kept for compatibility)
│   └── (same as steps/, will be deprecated)
└── wiki/                       # Wiki documentation
    ├── 00-HOME.md              # Home page with menu
    ├── 01-PRE-INSTALLATION.md  # Pre-installation steps
    ├── 02-CORE-INSTALLATION.md # Core installation (DEFAULT VIEW)
    ├── 03-POST-INSTALLATION.md # Post-installation configuration
    ├── COMPLETE-INSTALLATION.md # Complete guide (all steps)
    └── INSTALLATION-FLOWS.md   # Pre-built installation scenarios
```

---

## Contributing

Each module is independent. To add a new module:

1. Create `XX-module-name.md` in `modules/` directory
2. Follow the module structure (Purpose, Prerequisites, Time, Steps, Success, Next)
3. Update this README with the new module
4. Number modules logically (00-99)

---

**License:** This documentation is provided for educational purposes.

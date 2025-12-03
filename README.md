# Arch Linux Modular Installation System

**Repository:** https://github.com/merneo/arch-linux

This repository provides a **modular installation system** for Arch Linux. Each component is a separate module that can be used independently or combined as needed.

---

## 🚀 Quick Start

**Start here:** [Core Installation](wiki/02-CORE-INSTALLATION.md) - **Default view**

**Or browse:**
- [Home](wiki/00-HOME.md) - Installation menu
- [Complete Guide](wiki/COMPLETE-INSTALLATION.md) - All steps in one document
- [Installation Flows](wiki/INSTALLATION-FLOWS.md) - Pre-built installation scenarios

---

## Philosophy

**Modularity First:** Each installation step is a separate module. Pick and choose what you need.

**No Assumptions:** This system assumes you're already booted from Arch Linux Live USB. It doesn't format disks or create USB drives - it only installs and configures the system.

**Core is Minimal:** Core installation = pacstrap + root password. Nothing else.

**Flexibility:** Combine modules in any order to create your custom installation procedure.

---

## Available Modules

### Core Installation (Required)
- **`00-core-installation.md`** - Install base Arch Linux system (pacstrap) - **CORE MODULE**
- **`01-chroot.md`** - Enter chroot environment - **REQUIRED for configuration**

### System Configuration
- **`02-locale.md`** - Set locale and timezone
- **`03-root-password.md`** - Set root password
- **`04-user-creation.md`** - Create user account with sudo
- **`05-grub.md`** - Install GRUB bootloader

### Disk and Filesystem (Optional - Only if needed)
- **`06-disk-partitioning.md`** - Create disk partitions (ONLY if disk is not already partitioned)
- **`07-luks-encryption.md`** - Encrypt partitions with LUKS2
- **`08-btrfs-filesystem.md`** - Create Btrfs filesystem with subvolumes
- **`09-mount-partitions.md`** - Mount partitions (non-Btrfs)

### Advanced Configuration
- **`10-luks-keyfile-auto-unlock.md`** - Automatic LUKS decryption on boot
- **`11-networkmanager.md`** - Enable NetworkManager
- **`12-wifi.md`** - WiFi support
- **`13-bluetooth.md`** - Bluetooth support
- **`14-audio.md`** - Audio server (PipeWire)

### Security and Network (Optional)
- **`21-ssh-server.md`** - SSH server (port 1991, root disabled)
- **`22-ufw-firewall.md`** - UFW firewall (outgoing allowed, SSH incoming)
- **`23-fail2ban.md`** - Fail2ban for SSH protection

### Laptop Hardware (Laptops Only)
- **`17-laptop-touchpad.md`** - Configure touchpad/trackpad
- **`18-laptop-webcam.md`** - Configure built-in webcam
- **`19-laptop-ir-camera.md`** - Configure IR camera for face recognition
- **`20-laptop-fingerprint.md`** - Configure fingerprint reader

**Note:** Desktop computers typically don't have these built-in. Skip these modules for desktop installations.

### Final Steps
- **`15-exit-chroot.md`** - Exit chroot, unmount, reboot

---

## Quick Reference

- **`INSTALLATION-SCENARIOS.md`** - **NEW!** Choose installation scenario (dual boot, single boot, with/without LUKS)
- **`MODULE_INDEX.md`** - Detailed module reference and installation flows
- **`DESKTOP_VS_LAPTOP.md`** - Guide to choose modules based on hardware type (Desktop vs Laptop)
- **`GENERATE_PROCEDURE.md`** - How to generate custom installation procedure

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

### Module Documentation (Technical)

- **[Module Index](MODULE_INDEX.md)** - Quick reference for all modules
- **[Generate Procedure](GENERATE_PROCEDURE.md)** - How to generate custom procedure
- **[modules/](modules/)** - Individual module files

### AI Helpers

- **Helper:** `~/Documents/helpers/arch-linux-modular-installation.md` - Quick reference for AI assistants

---

## Repository Structure

```
arch-linux/
├── README.md                    # This file
├── MODULE_INDEX.md             # Quick reference for all modules
├── GENERATE_PROCEDURE.md       # How to generate custom procedure
├── modules/                    # Individual modules (23 modules)
│   ├── 00-core-installation.md # CORE: Install base system
│   ├── 01-chroot.md            # Enter chroot
│   ├── 02-locale.md            # Set locale and timezone
│   ├── 03-root-password.md     # Set root password
│   ├── 04-user-creation.md     # Create user account
│   ├── 05-grub.md              # Install GRUB bootloader
│   ├── 06-disk-partitioning.md # OPTIONAL: Create partitions
│   ├── 07-luks-encryption.md   # OPTIONAL: Encrypt partitions
│   ├── 08-btrfs-filesystem.md  # OPTIONAL: Create Btrfs
│   ├── 09-mount-partitions.md  # OPTIONAL: Mount partitions
│   ├── 10-luks-keyfile-auto-unlock.md # OPTIONAL: Auto-unlock
│   ├── 11-networkmanager.md    # Enable NetworkManager
│   ├── 12-wifi.md              # WiFi support
│   ├── 13-bluetooth.md         # Bluetooth support
│   ├── 14-audio.md             # Audio server
│   ├── 15-exit-chroot.md       # Exit and reboot
│   ├── 17-laptop-touchpad.md   # Laptop: Touchpad configuration
│   ├── 18-laptop-webcam.md     # Laptop: Webcam configuration
│   ├── 19-laptop-ir-camera.md  # Laptop: IR camera configuration
│   ├── 20-laptop-fingerprint.md # Laptop: Fingerprint reader
│   ├── 21-ssh-server.md        # SSH server (port 1991, root disabled)
│   ├── 22-ufw-firewall.md      # UFW firewall
│   └── 23-fail2ban.md          # Fail2ban SSH protection
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

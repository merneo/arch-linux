# Arch Linux Installation Guide - Main Index

**Welcome!** This is your starting point for installing Arch Linux. Choose your hardware and configuration below to get customized installation instructions.

---

## 🖥️ Choose Your Hardware Configuration

### Desktop Computer

#### Intel CPU
- **With NVIDIA GPU** → [Desktop: Intel CPU + NVIDIA GPU](#desktop-intel-nvidia)
- **With AMD GPU** → [Desktop: Intel CPU + AMD GPU](#desktop-intel-amd)
- **Intel Integrated Graphics** → [Desktop: Intel CPU + Integrated Graphics](#desktop-intel-integrated)

#### AMD CPU
- **With NVIDIA GPU** → [Desktop: AMD CPU + NVIDIA GPU](#desktop-amd-nvidia)
- **With AMD GPU** → [Desktop: AMD CPU + AMD GPU](#desktop-amd-amd)
- **AMD Integrated Graphics** → [Desktop: AMD CPU + Integrated Graphics](#desktop-amd-integrated)

### Laptop

#### Intel CPU
- **With NVIDIA GPU** → [Laptop: Intel CPU + NVIDIA GPU](#laptop-intel-nvidia)
- **Intel Integrated Graphics** → [Laptop: Intel CPU + Integrated Graphics](#laptop-intel-integrated)
- **With Fingerprint/Biometrics** → [Laptop: Intel + Biometrics](#laptop-intel-biometrics)

#### AMD CPU
- **With NVIDIA GPU** → [Laptop: AMD CPU + NVIDIA GPU](#laptop-amd-nvidia)
- **AMD Integrated Graphics** → [Laptop: AMD CPU + Integrated Graphics](#laptop-amd-integrated)
- **With Fingerprint/Biometrics** → [Laptop: AMD + Biometrics](#laptop-amd-biometrics)

---

## 🔧 Choose Your Installation Type

### Single Boot (Arch Linux Only)

#### With Encryption (LUKS)
- **Btrfs Filesystem** → [Encrypted Single Boot with Btrfs](wiki/INSTALLATION-FLOWS.md#encrypted-single-boot)
- **ext4/xfs Filesystem** → [Encrypted Single Boot with ext4](wiki/INSTALLATION-FLOWS.md#encrypted-single-boot-ext4)

#### Without Encryption
- **Btrfs Filesystem** → [Non-Encrypted Single Boot with Btrfs](wiki/INSTALLATION-FLOWS.md#non-encrypted-single-boot)
- **ext4/xfs Filesystem** → [Non-Encrypted Single Boot with ext4](wiki/INSTALLATION-FLOWS.md#non-encrypted-single-boot-ext4)

#### Minimal Installation
- **Minimal Setup** → [Minimal Installation](wiki/INSTALLATION-FLOWS.md#minimal-installation)

### Dual Boot (Arch Linux + Windows)

#### With Encryption
- **Encrypted Dual Boot** → [Dual Boot Encrypted](wiki/INSTALLATION-FLOWS.md#dual-boot-encrypted)

#### Without Encryption
- **Standard Dual Boot** → [Dual Boot Standard](wiki/INSTALLATION-FLOWS.md#dual-boot-standard)

---

## 📋 Installation Phases (Recommended - Follow in Order)

All phases are linked together with Previous/Next navigation:

1. **[Phase 00: Preparation](phases/PREPARATION.md)** → USB creation, Windows installation, disk formatting
2. **[Phase 01: Disk Setup](phases/DISK_SETUP.md)** → Partitioning, encryption, filesystem
3. **[Phase 02: System Install](phases/SYSTEM_INSTALL.md)** → Base system installation, chroot
4. **[Phase 03: Basic Config](phases/BASIC_CONFIG.md)** → Locale, user creation, sudo
5. **[Phase 04: Bootloader](phases/BOOTLOADER.md)** → GRUB installation, LUKS auto-unlock
6. **[Phase 05: Network](phases/NETWORK.md)** → NetworkManager, WiFi, Bluetooth
7. **[Phase 06: Audio](phases/AUDIO.md)** → PipeWire audio
8. **[Phase 07: Security](phases/SECURITY.md)** → SSH, UFW, fail2ban
9. **[Phase 08: Hardware](phases/HARDWARE.md)** → Touchpad, webcam, IR camera, fingerprint (laptops)
10. **[Phase 09: Finalize](phases/FINALIZE.md)** → Exit chroot, reboot

**Note:** Each phase has Previous/Next navigation links at the bottom for easy progression.

---

## 🎨 Choose Your Desktop Environment

### Desktop Environments
- **[GNOME](modules/gnome.md)** - Modern, user-friendly (Wayland/Xorg)
- **[KDE Plasma](modules/kde-plasma.md)** - Highly customizable (Wayland/Xorg)
- **[XFCE](modules/xfce.md)** - Lightweight, traditional (Xorg)

### Window Managers
- **[i3 Window Manager](modules/i3wm.md)** - Tiling WM (Xorg)
- **[Hyprland](modules/hyprland.md)** - Wayland compositor

---

## 📚 Complete Documentation

- **[Complete Installation Guide](wiki/COMPLETE-INSTALLATION.md)** - All steps in one document
- **[Installation Scenarios](INSTALLATION-SCENARIOS.md)** - Detailed scenarios by hardware
- **[Installation Flows](wiki/INSTALLATION-FLOWS.md)** - Pre-built installation flows
- **[Module Index](MODULE_INDEX.md)** - All available modules
- **[Step Index](STEP_INDEX.md)** - All individual steps
- **[Module Dependencies](MODULE_DEPENDENCIES.md)** - Dependency relationships
- **[Module Finder](MODULE_FINDER.md)** - Find modules by use case
- **[Quick Reference](QUICK_REFERENCE.md)** - Cheat sheet

---

## 🚀 Recommended Installation Paths

### Path 1: Desktop (Intel/NVIDIA) - Encrypted
```
Phase 00: Preparation
    ↓
Phase 01: Disk Setup (LUKS + Btrfs)
    ↓
Phase 02: System Install
    ↓
Phase 03: Basic Config
    ↓
Phase 04: Bootloader (with auto-unlock)
    ↓
Phase 05: Network
    ↓
Phase 09: Finalize
    ↓
[After Boot] NVIDIA Drivers → Desktop Environment
```

### Path 2: Laptop (AMD) - Encrypted with Biometrics
```
Phase 00: Preparation
    ↓
Phase 01: Disk Setup (LUKS + Btrfs)
    ↓
Phase 02: System Install
    ↓
Phase 03: Basic Config
    ↓
Phase 04: Bootloader (with auto-unlock)
    ↓
Phase 05: Network (WiFi)
    ↓
Phase 06: Audio
    ↓
Phase 07: Security
    ↓
Phase 08: Hardware (Touchpad, Webcam, Fingerprint)
    ↓
Phase 09: Finalize
    ↓
[After Boot] AMD Drivers → Desktop Environment
```

### Path 3: Dual Boot (Windows + Arch)
```
Phase 00: Preparation (Windows first)
    ↓
Phase 01: Disk Setup (Dual boot partitioning)
    ↓
Phase 02: System Install
    ↓
Phase 03: Basic Config
    ↓
Phase 04: Bootloader (with os-prober for Windows)
    ↓
Phase 05: Network
    ↓
Phase 09: Finalize
```

---

## 🔍 Quick Navigation by Component

### CPU
- **Intel** → All Intel-based configurations
- **AMD** → All AMD-based configurations

### GPU
- **NVIDIA** → [NVIDIA Drivers](modules/nvidia-drivers.md)
- **AMD** → [AMD Drivers](modules/amd-drivers.md)
- **Intel Integrated** → Usually works out of the box

### Storage
- **SATA** → Standard SATA drives
- **NVMe** → NVMe SSDs (faster)

### Network
- **Ethernet Only** → [NetworkManager](modules/networkmanager.md)
- **WiFi** → [NetworkManager](modules/networkmanager.md) + [WiFi](modules/wifi.md)
- **Bluetooth** → [Bluetooth](modules/bluetooth.md)

### Security
- **Full Stack** → [SSH](modules/ssh-server.md) + [UFW](modules/ufw-firewall.md) + [Fail2ban](modules/fail2ban.md)
- **Minimal** → [UFW](modules/ufw-firewall.md) only

---

## 📖 Repository Structure

- **`phases/`** - Organized installation phases with Previous/Next navigation (recommended)
- **`modules/`** - Detailed installation modules with explanations
- **`steps/`** - Individual command steps (quick reference)
- **`wiki/`** - Complete documentation and guides

---

## 🎯 Getting Started

1. **Choose your hardware type** above (Desktop/Laptop, Intel/AMD, GPU type)
2. **Choose your installation type** (Single boot/Dual boot, Encrypted/Non-encrypted)
3. **Follow the phases** in order - each phase links to the next
4. **After first boot**, install GPU drivers and desktop environment

---

**Start Here:** [Phase 00: Preparation](phases/PREPARATION.md) or choose a specific scenario above.

**Need Help?** Check [Installation Scenarios](INSTALLATION-SCENARIOS.md) for detailed hardware-specific guides.

---

**Repository:** https://github.com/merneo/arch-linux

# Arch Linux Installation Guides

**Repository:** https://github.com/merneo/arch-linux

This repository contains three versions of Arch Linux installation guides, each in a separate branch:

## Branches

### `core` - Vendor-Neutral Installation

Complete Arch Linux installation guide without vendor-specific hardware drivers.

**Features:**
- LUKS2 full-disk encryption
- Btrfs filesystem with subvolumes
- GRUB bootloader
- Minimal window manager setup
- **No SSH steps** - direct pacstrap installation from USB
- Vendor-neutral (no Intel/AMD specific drivers)

**Use this if:** You want a clean, vendor-neutral installation template.

---

### `core-intel` - Intel Hardware

Arch Linux installation guide with Intel CPU microcode and graphics drivers.

**Features:**
- Everything from `core` branch
- **Intel CPU microcode** (`intel-ucode`)
- **Intel graphics drivers** (Mesa, Vulkan, VA-API)
- Intel HD Graphics, Intel Iris, Intel Arc support

**Use this if:** You have Intel processor and integrated/discrete Intel graphics.

---

### `core-amd` - AMD Hardware

Arch Linux installation guide with AMD CPU microcode and graphics drivers.

**Features:**
- Everything from `core` branch
- **AMD CPU microcode** (`amd-ucode`)
- **AMD graphics drivers** (Mesa, Vulkan, VA-API)
- AMD Radeon integrated/discrete graphics support

**Use this if:** You have AMD processor and Radeon graphics (integrated or discrete).

---

## Installation Phases

All versions follow the same structure (11 phases):

1. **Phase 1:** Arch Linux Live USB Preparation + Network Connection
2. **Phase 2:** Disk Partitioning
3. **Phase 3:** LUKS2 Encryption Setup
4. **Phase 4:** Btrfs Filesystem Creation
5. **Phase 5:** Base System Installation (pacstrap)
6. **Phase 6:** System Configuration (Chroot)
7. **Phase 7:** Bootloader Installation (GRUB)
8. **Phase 8:** Automatic LUKS Decryption Setup (Optional)
9. **Phase 9:** User Account Creation
10. **Phase 10:** Network Configuration
11. **Phase 11:** Window Manager Setup (Optional)

**Note:** Phase 2 (SSH) has been removed. All commands run directly on the target system console.

---

## Quick Start

**1. Choose your branch:**
```bash
# For vendor-neutral installation
git checkout core

# For Intel hardware
git checkout core-intel

# For AMD hardware
git checkout core-amd
```

**2. Follow INSTALLATION.md:**
- Read `INSTALLATION.md` in the selected branch
- Follow step-by-step instructions
- All commands run directly on target system (no SSH required)

---

## Differences from Original

**Removed:**
- Phase 2: Remote Installation via SSH (Optional)
- `openssh` package from pacstrap
- Step 12.8: Enable SSH Service

**Added:**
- Step 1.6: Connect to Network (in Phase 1)
- Direct pacstrap installation from USB

**Result:** Cleaner installation process focused on direct USB installation.

---

## Repository Structure

```
arch-linux/
├── core/          → INSTALLATION.md (vendor-neutral)
├── core-intel/    → INSTALLATION.md (Intel hardware)
└── core-amd/      → INSTALLATION.md (AMD hardware)
```

---

## Related Documentation

- **Helper:** `~/Documents/helpers/universal-core-arch-installation.md` - Quick reference guide
- **Source:** Based on `~/EliteBook/docs/installation/blank_arch.md` (modified to remove SSH)

---

**License:** This documentation is provided for educational purposes.

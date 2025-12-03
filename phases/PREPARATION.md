# Phase 00: Preparation

**Purpose:** Prepare USB, install Windows, and optionally format disk before proceeding with the Arch Linux installation. This phase covers essential groundwork to ensure a smooth setup. For a general overview of the pre-installation process, refer to the [ArchWiki Installation Guide#Pre-installation](https://wiki.archlinux.org/title/Installation_guide#Pre-installation).

**Prerequisites:**
- Second computer (preparation machine) with internet connection
- USB flash drive (8 GB minimum)
- Target computer ready

**Steps (execute in order):**

1. **[Create Arch Linux USB](../steps/PRE-01-create-arch-usb.md)**
   - Download Arch Linux ISO
   - Create bootable USB drive (Linux, Windows, or macOS)

2. **[Install Windows](../steps/PRE-02-install-windows.md)** (dual boot only)
   - Install Windows first (if dual booting)
   - Configure Windows for dual boot (disable Fast Startup, BitLocker)
   - Leave free space for Arch Linux

3. **[Format Disk](../steps/PRE-03-format-disk.md)** (fresh install only)
   - Completely wipe disk (optional, if you want fresh start)
   - Only use if you want to remove all existing data

## Verification

- [ ] Arch Linux USB created and tested
- [ ] Windows installed (if dual booting)
- [ ] Windows configured for dual boot (if dual booting)
- [ ] Disk formatted (if fresh install)

## Next Phase

**[Phase 01: Disk Setup](DISK_SETUP.md)**

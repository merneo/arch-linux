# Phase 00: Preparation

**Purpose:** Prepare USB, install Windows, and optionally format disk before proceeding with the Arch Linux installation. This phase covers essential groundwork to ensure a smooth setup. For a general overview of the pre-installation process, refer to the [ArchWiki Installation Guide#Pre-installation](https://wiki.archlinux.org/title/Installation_guide#Pre-installation).

**Time Estimate:** 30-60 minutes  
**Difficulty:** ⭐ Easy

**Prerequisites:**
- Second computer (preparation machine) with internet connection
- USB flash drive (8 GB minimum)
- Target computer ready

**Steps (execute in order):**

1. **[Create Arch Linux USB](../steps/pre-create-arch-usb-commands.md)**
   - Download Arch Linux ISO
   - Create bootable USB drive (Linux, Windows, or macOS)

2. **[Install Windows](../steps/pre-install-windows-commands.md)** (dual boot only)
   - Install Windows first (if dual booting)
   - Configure Windows for dual boot (disable Fast Startup, BitLocker)
   - Leave free space for Arch Linux

3. **[Format Disk](../steps/pre-format-disk-commands.md)** (fresh install only)
   - Completely wipe disk (optional, if you want fresh start)
   - Only use if you want to remove all existing data

## Verification

- [ ] Arch Linux USB created and tested
- [ ] Windows installed (if dual booting)
- [ ] Windows configured for dual boot (if dual booting)
- [ ] Disk formatted (if fresh install)

---

## Navigation

**Previous:** This is the first phase (no previous phase)

**Next:** **[Phase 01: Disk Setup →](01-disk-setup.md)**

---

**Back to:** [Main Index](../index.md) | [Repository Root](../README.md)

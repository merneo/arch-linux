# Phase 02: System Install

**Purpose:** Install base Arch Linux system and enter chroot. This phase lays the foundation of your Arch Linux installation, transferring the core operating system files to your disk and preparing the environment for further configuration. For a detailed understanding of these steps, refer to the [ArchWiki Installation Guide#Install essential packages](https://wiki.archlinux.org/title/Installation_guide#Install_essential_packages) and the [ArchWiki on arch-chroot](https://wiki.archlinux.org/title/Arch-chroot).

**Prerequisites:**
- Disk partitions created and mounted at /mnt
- Network connection established

**Steps (execute in order):**

1. **[Core System Installation](../steps/core-installation.md)**
   - Install base system with pacstrap
   - Generate fstab

2. **[Enter Chroot](../steps/chroot.md)**
   - Enter chroot environment for configuration

## Verification

```bash
# Check system installed
ls -la /mnt/bin /mnt/etc /mnt/usr

# Verify in chroot
arch-chroot /mnt
```

## Next Phase

**[Phase 03: Basic Config](BASIC_CONFIG.md)**

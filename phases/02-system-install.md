# Phase 02: System Install

**Purpose:** Install base Arch Linux system and enter chroot. This phase lays the foundation of your Arch Linux installation, transferring the core operating system files to your disk and preparing the environment for further configuration. For a detailed understanding of these steps, refer to the [ArchWiki Installation Guide#Install essential packages](https://wiki.archlinux.org/title/Installation_guide#Install_essential_packages) and the [ArchWiki on arch-chroot](https://wiki.archlinux.org/title/Arch-chroot).

**Prerequisites:**
- Disk partitions created and mounted at /mnt
- Network connection established

**Time Estimate:** 10-20 minutes
**Difficulty:** ⭐ Easy

**Steps (execute in order):**

1. **[Core System Installation](../steps/core-installation-commands.md)**
   - Install base system with pacstrap
   - Generate fstab

2. **[Enter Chroot](../steps/chroot-commands.md)**
   - Enter chroot environment for configuration

## Verification

```bash
# Check system installed
ls -la /mnt/bin /mnt/etc /mnt/usr

# Verify in chroot
arch-chroot /mnt
```

---

## Navigation

**Previous:** **[← Phase 01: Disk Setup](01-disk-setup.md)**

**Next:** **[Phase 03: Basic Config →](03-basic-config.md)**

---

**Back to:** [Main Index](../index.md) | [Repository Root](../README.md)

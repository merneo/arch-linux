# Phase 02: System Install

**Purpose:** Install base Arch Linux system and enter chroot

**Prerequisites:**
- Disk partitions created and mounted at /mnt
- Network connection established

**Steps (execute in order):**

1. **[Core System Installation](../steps/00-core-installation.md)**
   - Install base system with pacstrap
   - Generate fstab

2. **[Enter Chroot](../steps/01-chroot.md)**
   - Enter chroot environment for configuration

## Verification

```bash
# Check system installed
ls -la /mnt/bin /mnt/etc /mnt/usr

# Verify in chroot
arch-chroot /mnt
```

## Next Phase

**[Phase 03: Basic Config](03-BASIC_CONFIG.md)**

# Phase 04: Bootloader

**Purpose:** Install GRUB bootloader and configure auto-unlock (if using LUKS). The bootloader is essential for initiating the operating system after the system's firmware completes its POST (Power-On Self-Test). For general information on boot loaders, refer to the [ArchWiki on Boot loaders](https://wiki.archlinux.org/title/Boot_loaders), and specifically for GRUB, see the [ArchWiki on GRUB](https://wiki.archlinux.org/title/GRUB).

**Prerequisites:**
- Inside chroot environment
- EFI partition mounted at /boot

**Time Estimate:** 10-15 minutes
**Difficulty:** ⭐⭐ Intermediate

**Steps (execute in order):**

1. **[GRUB Installation](../steps/grub-commands.md)**
   - Install GRUB to EFI partition
   - Configure GRUB (standard, LUKS, or LUKS+Btrfs)
   - Generate GRUB configuration

2. **[LUKS Auto-Unlock](../steps/luks-keyfile-auto-unlock-commands.md)** (if using LUKS)
   - Create keyfile for automatic LUKS decryption
   - Configure initramfs and GRUB

## Verification

```bash
ls -la /boot/grub/grub.cfg
ls -la /boot/EFI/GRUB/
grep "GRUB_CMDLINE_LINUX" /etc/default/grub
```

---

## Navigation

**Previous:** **[← Phase 03: Basic Config](03-basic-config.md)**

**Next:** 
- **[Phase 05: Network →](05-network.md)** (optional - if you need network)
- **[Phase 09: Finalize →](09-finalize.md)** (if skipping network/audio/security)

---

**Back to:** [Main Index](../index.md) | [Repository Root](../README.md)

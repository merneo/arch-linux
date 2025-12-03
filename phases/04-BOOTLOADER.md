# Phase 04: Bootloader

**Purpose:** Install GRUB bootloader and configure auto-unlock (if using LUKS)

**Prerequisites:**
- Inside chroot environment
- EFI partition mounted at /boot

**Steps (execute in order):**

1. **[GRUB Installation](../steps/05-grub.md)**
   - Install GRUB to EFI partition
   - Configure GRUB (standard, LUKS, or LUKS+Btrfs)
   - Generate GRUB configuration

2. **[LUKS Auto-Unlock](../steps/10-luks-keyfile-auto-unlock.md)** (if using LUKS)
   - Create keyfile for automatic LUKS decryption
   - Configure initramfs and GRUB

## Verification

```bash
ls -la /boot/grub/grub.cfg
ls -la /boot/EFI/GRUB/
grep "GRUB_CMDLINE_LINUX" /etc/default/grub
```

## Next Phase

**[Phase 05: Network](05-NETWORK.md)** (optional) or **[Phase 09: Finalize](09-FINALIZE.md)**

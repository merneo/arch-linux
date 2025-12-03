# Step: GRUB Bootloader Installation

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment, EFI partition mounted at /boot

## Commands

```bash
# Install GRUB
pacman -S grub efibootmgr os-prober

# Install GRUB to EFI partition
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB --recheck

# Get root partition UUID (replace <ROOT_PARTITION> with your root partition)
blkid <ROOT_PARTITION>
# Copy the UUID

# Configure GRUB
nano /etc/default/grub
```

**Edit `/etc/default/grub`:**

**For standard installation (no encryption):**
```bash
GRUB_CMDLINE_LINUX="root=UUID=<YOUR_UUID>"
GRUB_DISABLE_OS_PROBER=false
```

**For LUKS encryption:**
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<LUKS_UUID>:cryptroot root=/dev/mapper/cryptroot"
GRUB_DISABLE_OS_PROBER=false
```

**For LUKS + Btrfs:**
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<LUKS_UUID>:cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@"
GRUB_DISABLE_OS_PROBER=false
```

**Generate GRUB configuration:**
```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

## Verification

```bash
ls -la /boot/grub/grub.cfg
ls -la /boot/EFI/GRUB/
grep "GRUB_CMDLINE_LINUX" /etc/default/grub
```

**NEXT:** `10-luks-keyfile-auto-unlock.md` (if using LUKS auto-unlock) or `15-exit-chroot.md`

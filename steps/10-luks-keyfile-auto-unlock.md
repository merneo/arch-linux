# Step: LUKS Keyfile Auto-Unlock

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment, LUKS encryption configured

## Commands

```bash
# Create keyfile directory
mkdir -p /etc/cryptsetup.d
chmod 700 /etc/cryptsetup.d

# Generate keyfile
dd if=/dev/urandom of=/etc/cryptsetup.d/root.key bs=512 count=1
chmod 600 /etc/cryptsetup.d/root.key

# Add keyfile to LUKS (replace /dev/sdX2 with your root partition)
cryptsetup luksAddKey /dev/sdX2 /etc/cryptsetup.d/root.key
# Enter existing passphrase

# Add keyfile to swap (if encrypted, replace /dev/sdX3)
cryptsetup luksAddKey /dev/sdX3 /etc/cryptsetup.d/root.key

# Configure initramfs
nano /etc/mkinitcpio.conf
# Change: FILES=(/etc/cryptsetup.d/root.key)
# Add encrypt hook: HOOKS=(base udev autodetect modconf block encrypt filesystems keyboard fsck)

# Update GRUB
nano /etc/default/grub
# Add cryptkey parameter: GRUB_CMDLINE_LINUX="cryptkey=rootfs:/etc/cryptsetup.d/root.key cryptdevice=UUID=<YOUR_UUID>:cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@"

# Configure encrypted swap auto-unlock
echo "cryptswap /dev/sdX3 /etc/cryptsetup.d/root.key luks" > /etc/crypttab

# Rebuild initramfs and GRUB
mkinitcpio -P
grub-mkconfig -o /boot/grub/grub.cfg
```

## Verification

```bash
cryptsetup luksDump /dev/sdX2 | grep "Key Slot"
# Should show: Key Slot 0: ENABLED, Key Slot 1: ENABLED
```

**NEXT:** `15-exit-chroot.md`

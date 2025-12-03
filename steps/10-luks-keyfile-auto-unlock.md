# Step: LUKS Keyfile Auto-Unlock

**Purpose:** Configure automatic LUKS decryption on boot using keyfile. For a detailed guide on LUKS keyfiles, refer to the [ArchWiki on dm-crypt/Encrypting an entire system#Keyfiles](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#Keyfiles) and the role of [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio) in this process.

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment, LUKS encryption configured

## Commands

```bash
# Create keyfile directory
mkdir -p /etc/cryptsetup.d
chmod 700 /etc/cryptsetup.d

# Generate keyfile. See [ArchWiki: Random number generation](https://wiki.archlinux.org/title/Random_number_generation) for /dev/urandom.
dd if=/dev/urandom of=/etc/cryptsetup.d/root.key bs=512 count=1
chmod 600 /etc/cryptsetup.d/root.key

# Add keyfile to LUKS (replace /dev/sdX2 with your root partition). See [ArchWiki: dm-crypt/Encrypting an entire system#Keyfiles](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#Keyfiles).
cryptsetup luksAddKey /dev/sdX2 /etc/cryptsetup.d/root.key
# Enter existing passphrase

# Add keyfile to swap (if encrypted, replace /dev/sdX3)
cryptsetup luksAddKey /dev/sdX3 /etc/cryptsetup.d/root.key

# Configure initramfs. See [ArchWiki: mkinitcpio#Adding a keyfile](https://wiki.archlinux.org/title/Mkinitcpio#Adding_a_keyfile).
nano /etc/mkinitcpio.conf
# Change: FILES=(/etc/cryptsetup.d/root.key)
# Add encrypt hook: HOOKS=(base udev autodetect modconf block encrypt filesystems keyboard fsck)

# Update GRUB. See [ArchWiki: GRUB#Encrypting_an_entire_system](https://wiki.archlinux.org/title/GRUB#Encrypting_an_entire_system) for cryptkey parameter.
nano /etc/default/grub
# Add cryptkey parameter: GRUB_CMDLINE_LINUX="cryptkey=rootfs:/etc/cryptsetup.d/root.key cryptdevice=UUID=<YOUR_UUID>:cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@"

# Configure encrypted swap auto-unlock. See [ArchWiki: crypttab](https://wiki.archlinux.org/title/Dm-crypt/System_configuration#crypttab) and [ArchWiki: dm-crypt/Swap encryption](https://wiki.archlinux.org/title/Dm-crypt/Swap_encryption).
echo "cryptswap /dev/sdX3 /etc/cryptsetup.d/root.key luks" > /etc/crypttab

# Rebuild initramfs and GRUB. See [ArchWiki: mkinitcpio#Image generation](https://wiki.archlinux.org/title/Mkinitcpio#Image_generation) and [ArchWiki: GRUB/Configuration](https://wiki.archlinux.org/title/GRUB/Configuration).
mkinitcpio -P
grub-mkconfig -o /boot/grub/grub.cfg
```

## Verification

```bash
cryptsetup luksDump /dev/sdX2 | grep "Key Slot" # Verify keyfile was added. See [ArchWiki: dm-crypt/Encrypting an entire system#Keyfiles](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#Keyfiles).
# Should show: Key Slot 0: ENABLED, Key Slot 1: ENABLED
```

**NEXT:** `15-exit-chroot.md`

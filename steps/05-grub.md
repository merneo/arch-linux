# Step: GRUB Bootloader Installation

**Purpose:** Install and configure GRUB bootloader. For comprehensive information, refer to the [ArchWiki on GRUB](https://wiki.archlinux.org/title/GRUB).

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment, EFI partition mounted at /boot

## Commands

```bash
# Install GRUB. See [ArchWiki: GRUB#Installation](https://wiki.archlinux.org/title/GRUB#Installation).
pacman -S grub efibootmgr os-prober

# Install GRUB to EFI partition. For UEFI specific installation, see [ArchWiki: GRUB#UEFI_systems](https://wiki.archlinux.org/title/GRUB#UEFI_systems).
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB --recheck

# Get root partition UUID (replace <ROOT_PARTITION> with your root partition).
# Using UUIDs for identifying partitions is recommended for [Persistent block device naming](https://wiki.archlinux.org/title/Persistent_block_device_naming).
blkid <ROOT_PARTITION>
# Copy the UUID

# Configure GRUB. This involves editing the '/etc/default/grub' file.
# For details on GRUB configuration, see [ArchWiki: GRUB/Configuration](https://wiki.archlinux.org/title/GRUB/Configuration).
nano /etc/default/grub
```

**Edit `/etc/default/grub`:** The `GRUB_CMDLINE_LINUX` variable passes kernel parameters during boot. For details on kernel parameters, refer to the [ArchWiki on Kernel parameters](https://wiki.archlinux.org/title/Kernel_parameters).

**For standard installation (no encryption):**
```bash
GRUB_CMDLINE_LINUX="root=UUID=<YOUR_UUID>"
GRUB_DISABLE_OS_PROBER=false
```

**For LUKS encryption:** For configuring GRUB for encrypted systems, see [ArchWiki: GRUB#Encrypting_an_entire_system](https://wiki.archlinux.org/title/GRUB#Encrypting_an_entire_system).
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<LUKS_UUID>:cryptroot root=/dev/mapper/cryptroot"
GRUB_DISABLE_OS_PROBER=false
```

**For LUKS + Btrfs:** When using Btrfs subvolumes for the root filesystem, specific `rootflags` are needed. See [ArchWiki: GRUB#Btrfs](https://wiki.archlinux.org/title/GRUB#Btrfs).
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<LUKS_UUID>:cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@"
GRUB_DISABLE_OS_PROBER=false
```

**Generate GRUB configuration:** The `grub-mkconfig` utility generates the main GRUB configuration file. See [ArchWiki: GRUB/Configuration#Generate the main configuration file](https://wiki.archlinux.org/title/GRUB/Configuration#Generate_the_main_configuration_file).
```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

## Verification

```bash
ls -la /boot/grub/grub.cfg
ls -la /boot/EFI/GRUB/
grep "GRUB_CMDLINE_LINUX" /etc/default/grub # Verify GRUB configuration. See [ArchWiki: GRUB/Configuration](https://wiki.archlinux.org/title/GRUB/Configuration).
```

**NEXT:** `10-luks-keyfile-auto-unlock.md` (if using LUKS auto-unlock) or `15-exit-chroot.md`

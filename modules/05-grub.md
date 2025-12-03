# Module: GRUB Bootloader Installation

**Purpose:** Install and configure GRUB bootloader. GRUB (GRand Unified Bootloader) is a boot loader package from the GNU Project. It is the default boot loader for Arch Linux installations and supports both BIOS and UEFI systems. For comprehensive information, refer to the [ArchWiki on GRUB](https://wiki.archlinux.org/title/GRUB).

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)
- EFI partition mounted at `/boot`
- Root partition UUID identified

**Time:** 10-15 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## Step 1: Install GRUB

This step installs the necessary GRUB packages and then installs GRUB to the EFI System Partition. For detailed information on these packages and the installation process, refer to the [ArchWiki on GRUB](https://wiki.archlinux.org/title/GRUB).

```bash
pacman -S grub efibootmgr os-prober
```

**Package breakdown:**
- `grub`: The GRUB bootloader package.
- `efibootmgr`: Used to manage UEFI boot entries. See [ArchWiki: efibootmgr](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface#efibootmgr).
- `os-prober`: Detects other operating systems (useful for dual-booting). See [ArchWiki: GRUB#Detecting other operating systems](https://wiki.archlinux.org/title/GRUB#Detecting_other_operating_systems).

```bash
# Install GRUB to EFI partition. For UEFI specific installation, see [ArchWiki: GRUB#UEFI_systems](https://wiki.archlinux.org/title/GRUB#UEFI_systems).
grub-install --target=x86_64-efi \
  --efi-directory=/boot \
  --bootloader-id=GRUB \
  --recheck
```

---

## Step 2: Get Root Partition UUID

Using UUIDs (Universally Unique Identifiers) for identifying partitions in GRUB and `fstab` is generally recommended over device names (like `/dev/sdaX`) because UUIDs are persistent and do not change if the hardware configuration changes. For more information, refer to the [ArchWiki on Persistent block device naming](https://wiki.archlinux.org/title/Persistent_block_device_naming) and the `blkid` utility.

```bash
# Display root partition UUID
# Replace <ROOT_PARTITION> with your actual root partition (e.g., /dev/sda2, /dev/nvme0n1p2)
blkid <ROOT_PARTITION>

# Or filter only UUID value
blkid -s UUID -o value <ROOT_PARTITION>
```

**Copy the UUID** (format: `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX`)

**Placeholders:**
- `<ROOT_PARTITION>`: Your root partition device (e.g., `/dev/sda2`, `/dev/nvme0n1p2`)
- `<YOUR_UUID>`: UUID from the command above

---

## Step 3: Configure GRUB

The `/etc/default/grub` file contains GRUB's configuration options. The `GRUB_CMDLINE_LINUX` variable passes kernel parameters during boot, which are crucial for defining root filesystem location, encryption, and other boot-time options. For a full list of kernel parameters, consult the [ArchWiki on Kernel parameters](https://wiki.archlinux.org/title/Kernel_parameters).

```bash
nano /etc/default/grub
```

**For standard installation (no encryption):**
```bash
GRUB_CMDLINE_LINUX="root=UUID=<YOUR_UUID>"
GRUB_DISABLE_OS_PROBER=false
```

**For LUKS encryption:** For details on configuring GRUB for encrypted systems, see [ArchWiki: GRUB#Encrypting_an_entire_system](https://wiki.archlinux.org/title/GRUB#Encrypting_an_entire_system).
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<LUKS_UUID>:cryptroot root=/dev/mapper/cryptroot"
GRUB_DISABLE_OS_PROBER=false
```

**For Btrfs subvolume:** When using Btrfs subvolumes for the root filesystem, special `rootflags` are needed. See [ArchWiki: GRUB#Btrfs](https://wiki.archlinux.org/title/GRUB#Btrfs).
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<LUKS_UUID>:cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@"
GRUB_DISABLE_OS_PROBER=false
```

**Placeholders:**
- `<YOUR_UUID>`: Root partition UUID from Step 2
- `<LUKS_UUID>`: LUKS encrypted partition UUID (get with `blkid <LUKS_PARTITION>`)

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

---

## Step 4: Generate GRUB Configuration

After modifying `/etc/default/grub`, the GRUB configuration file (`grub.cfg`) must be regenerated to apply the changes. The `grub-mkconfig` utility performs this task. For more details, refer to the [ArchWiki on GRUB/Configuration#Generate the main configuration file](https://wiki.archlinux.org/title/GRUB/Configuration#Generate_the_main_configuration_file).

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

**Expected output:**
```
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-linux
Found initrd image: /boot/initramfs-linux.img
done
```

---

**SUCCESS:** GRUB bootloader installed and configured

## Verification

Run these commands to verify GRUB installation:

```bash
# Check GRUB files exist
ls -la /boot/grub/grub.cfg
ls -la /boot/EFI/GRUB/

# Should show GRUB configuration files

# Verify GRUB configuration
grep "GRUB_CMDLINE_LINUX" /etc/default/grub

# Should show your root UUID or cryptdevice configuration
```

**Next:** Continue with other configuration modules or exit chroot

---

## Troubleshooting

For more extensive troubleshooting on GRUB issues, refer to the [ArchWiki on GRUB/Troubleshooting](https://wiki.archlinux.org/title/GRUB/Troubleshooting).

### Problem: grub-install fails with "cannot find EFI directory"
**Solution:**
1. Verify EFI partition is mounted: `mount | grep /boot`
2. Check EFI partition exists: `lsblk | grep -i efi`
3. Mount EFI partition: `mount /dev/<EFI_PARTITION> /boot` (if not mounted)

### Problem: System doesn't boot after GRUB installation
**Solution:**
1. Verify UUID in `/etc/default/grub` matches actual partition UUID: `blkid`
2. Regenerate GRUB config: `grub-mkconfig -o /boot/grub/grub.cfg`
3. Check GRUB config syntax: `grub-mkconfig -o /boot/grub/grub.cfg` (should not show errors)

### Problem: Wrong UUID in GRUB configuration
**Solution:**
1. Get correct UUID: `blkid <ROOT_PARTITION>`
2. Edit `/etc/default/grub`: `nano /etc/default/grub`
3. Update `GRUB_CMDLINE_LINUX` with correct UUID
4. Regenerate: `grub-mkconfig -o /boot/grub/grub.cfg`

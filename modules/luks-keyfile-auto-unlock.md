# Module: LUKS Keyfile Auto-Unlock

**Purpose:** Configure automatic LUKS decryption on boot using a keyfile. This method allows the system to unlock encrypted partitions without requiring manual passphrase entry at every boot, enhancing convenience. For a detailed guide on LUKS keyfiles, refer to the [ArchWiki on dm-crypt/Encrypting an entire system#Keyfiles](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#Keyfiles) and the role of [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio) in this process.

**Prerequisites:**
- Inside chroot environment (module `chroot.md`)
- LUKS encryption configured (module `luks-encryption.md`)
- GRUB configured (module `grub.md`)

**Time:** 10-15 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## Security Notes

- **Keyfile security:** The keyfile is embedded in initramfs, which means anyone with physical access to the disk can boot without a passphrase. This introduces a potential security vulnerability if physical access is compromised.
- **Trade-off:** Convenience vs. security - auto-unlock makes boot easier but reduces security, especially against cold boot attacks. Consider your threat model.
- **Backup keyfile:** Store a backup securely (not on the same disk).
- **Alternative:** For enhanced security with auto-unlock, consider using a [Trusted Platform Module (TPM)](https://wiki.archlinux.org/title/Trusted_Platform_Module) if your hardware supports it (an advanced configuration).

---

## Step 1: Create Keyfile Directory

```bash
mkdir -p /etc/cryptsetup.d
chmod 700 /etc/cryptsetup.d
```

---

## Step 2: Generate LUKS Keyfile

The `dd` command is used to copy data, and here it generates a random keyfile from `/dev/urandom`. `/dev/urandom` is a special file that serves as a non-blocking source of pseudo-random numbers. For more on these commands, consult `man dd` and `man urandom`, or see [ArchWiki: Random number generation](https://wiki.archlinux.org/title/Random_number_generation).

```bash
# Generate 512-byte random keyfile
dd if=/dev/urandom of=/etc/cryptsetup.d/root.key bs=512 count=1

# Set strict permissions
chmod 600 /etc/cryptsetup.d/root.key
```

---

## Step 3: Add Keyfile to LUKS Key Slot

Adding the keyfile to a LUKS key slot allows the encrypted volume to be unlocked using the keyfile instead of (or in addition to) a passphrase. For more information, refer to [ArchWiki: dm-crypt/Encrypting an entire system#Keyfiles](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#Keyfiles) and `man cryptsetup`.

```bash
# Replace /dev/sdX2 with your root partition
cryptsetup luksAddKey /dev/sdX2 /etc/cryptsetup.d/root.key
```

**Enter existing LUKS passphrase** when prompted.

**Verify keyfile was added:** The `cryptsetup luksDump` command displays LUKS header information, including which key slots are enabled.

```bash
cryptsetup luksDump /dev/sdX2 | grep "Key Slot"

# Should show:
# Key Slot 0: ENABLED
# Key Slot 1: ENABLED
```

---

## Step 4: Add Keyfile to Swap Partition (if encrypted)

```bash
# Replace /dev/sdX3 with your swap partition
cryptsetup luksAddKey /dev/sdX3 /etc/cryptsetup.d/root.key

# Enter existing passphrase
```

---

## Step 5: Configure Initramfs to Include Keyfile

The keyfile must be included in the [initramfs](https://wiki.archlinux.org/title/Mkinitcpio) so that it is available early in the boot process to unlock the root partition. The `encrypt` hook is also necessary to handle LUKS encryption. For detailed instructions, refer to [ArchWiki: mkinitcpio#Adding a keyfile](https://wiki.archlinux.org/title/Mkinitcpio#Adding_a_keyfile).

```bash
nano /etc/mkinitcpio.conf
```

**Find line:**
```bash
FILES=()
```

**Change to:**
```bash
FILES=(/etc/cryptsetup.d/root.key)
```

**Find HOOKS line:**
```bash
HOOKS=(base udev autodetect modconf block filesystems keyboard fsck)
```

**Add `encrypt` hook (MUST be after `block`, before `filesystems`):**
```bash
HOOKS=(base udev autodetect modconf block encrypt filesystems keyboard fsck)
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

---

## Step 6: Update GRUB for Keyfile

GRUB needs to be configured to use the keyfile for decryption at boot. This is done by adding the `cryptkey` parameter to `GRUB_CMDLINE_LINUX`. For a detailed explanation, refer to the [ArchWiki on GRUB#Encrypting_an_entire_system](https://wiki.archlinux.org/title/GRUB#Encrypting_an_entire_system) section.

```bash
nano /etc/default/grub
```

**Find line starting with `GRUB_CMDLINE_LINUX=`**

**Add `cryptkey` parameter:**
```bash
GRUB_CMDLINE_LINUX="cryptkey=rootfs:/etc/cryptsetup.d/root.key cryptdevice=UUID=<YOUR_UUID>:cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@"
```

**Replace `<YOUR_UUID>` with your root partition UUID**

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

---

## Step 7: Configure Encrypted Swap Auto-Unlock

For encrypted swap partitions to be automatically unlocked and activated at boot using the keyfile, entries must be added to `/etc/crypttab` and `/etc/fstab`. The `crypttab` file defines encrypted block devices that are set up during system boot. For detailed instructions, refer to the [ArchWiki on crypttab](https://wiki.archlinux.org/title/Dm-crypt/System_configuration#crypttab) and [ArchWiki on dm-crypt/Swap encryption](https://wiki.archlinux.org/title/Dm-crypt/Swap_encryption). For `fstab` configuration, see [ArchWiki on fstab](https://wiki.archlinux.org/title/Fstab).

```bash
# Create /etc/crypttab for swap
cat > /etc/crypttab << EOF
# <name>       <device>                             <keyfile>                          <options>
cryptswap      /dev/sdX3                            /etc/cryptsetup.d/root.key         luks
EOF
```

**Replace `/dev/sdX3` with your swap partition**

**Update fstab for encrypted swap:**
```bash
nano /etc/fstab
```

**Find swap line and change to:**
```
/dev/mapper/cryptswap  none  swap  defaults  0  0
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

---

## Step 8: Rebuild Initramfs and GRUB

After modifying `/etc/mkinitcpio.conf` and `/etc/default/grub`, both the initramfs image and the GRUB configuration file must be regenerated to incorporate the changes. For more details, refer to [ArchWiki: mkinitcpio#Image_generation](https://wiki.archlinux.org/title/Mkinitcpio#Image_generation) and [ArchWiki: GRUB/Configuration#Generate_the_main_configuration_file](https://wiki.archlinux.org/title/GRUB/Configuration#Generate_the_main_configuration_file).

```bash
# Regenerate initramfs (includes keyfile now)
mkinitcpio -P

# Regenerate GRUB config
grub-mkconfig -o /boot/grub/grub.cfg
```

---

## Step 9: Verify Keyfile is in Initramfs

It's crucial to verify that the keyfile has indeed been embedded into the initramfs image. The `lsinitcpio` utility lists the contents of an initramfs file. For more information, refer to [ArchWiki: mkinitcpio#Contents](https://wiki.archlinux.org/title/Mkinitcpio#Contents).

```bash
# List contents of initramfs
lsinitcpio /boot/initramfs-linux.img | grep root.key

# Should show:
# etc/cryptsetup.d/root.key
```

---

**SUCCESS:** Automatic LUKS decryption configured

**Note:** System will now boot without asking for LUKS passphrase (keyfile is in `/boot` partition)

---

## Troubleshooting

For more extensive troubleshooting on LUKS keyfiles, refer to the [ArchWiki on dm-crypt#Troubleshooting](https://wiki.archlinux.org/title/Dm-crypt#Troubleshooting).

### Problem: System still asks for passphrase on boot
**Solution:**
1. Verify keyfile is in initramfs: `lsinitcpio /boot/initramfs-linux.img | grep root.key`
2. Check GRUB config: `grep cryptkey /boot/grub/grub.cfg`
3. Verify keyfile was added to LUKS: `cryptsetup luksDump /dev/sdX2 | grep "Key Slot"`
4. Rebuild initramfs: `mkinitcpio -P`
5. Regenerate GRUB: `grub-mkconfig -o /boot/grub/grub.cfg`

### Problem: Keyfile not found in initramfs
**Solution:**
1. Check mkinitcpio.conf: `grep FILES /etc/mkinitcpio.conf`
2. Verify keyfile path is correct: `ls -la /etc/cryptsetup.d/root.key`
3. Ensure keyfile has correct permissions: `chmod 600 /etc/cryptsetup.d/root.key`
4. Rebuild initramfs: `mkinitcpio -P`

### Problem: cryptsetup luksAddKey fails
**Solution:**
1. Verify partition is correct: `lsblk` or `fdisk -l`
2. Check LUKS is already set up: `cryptsetup isLuks /dev/sdX2`
3. Ensure you're entering correct passphrase
4. Check for free key slot: `cryptsetup luksDump /dev/sdX2 | grep "Key Slot"`

### Problem: Encrypted swap not activating
**Solution:**
1. Check crypttab: `cat /etc/crypttab`
2. Verify fstab entry: `grep swap /etc/fstab`
3. Check swap status: `swapon --show`
4. Enable manually: `cryptsetup open /dev/sdX3 cryptswap --key-file /etc/cryptsetup.d/root.key && swapon /dev/mapper/cryptswap`

---

**Next:** Continue with other configuration modules

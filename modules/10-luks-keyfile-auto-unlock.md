# Module: LUKS Keyfile Auto-Unlock

**Purpose:** Configure automatic LUKS decryption on boot using keyfile

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)
- LUKS encryption configured (module `07-luks-encryption.md`)
- GRUB configured (module `05-grub.md`)

**Time:** 10-15 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## Security Notes

- **Keyfile security:** The keyfile is embedded in initramfs, which means anyone with physical access to the disk can boot without a passphrase
- **Trade-off:** Convenience vs. security - auto-unlock makes boot easier but reduces security
- **Backup keyfile:** Store a backup securely (not on the same disk)
- **Alternative:** Use TPM (Trusted Platform Module) for more secure auto-unlock (advanced)

---

## Step 1: Create Keyfile Directory

```bash
mkdir -p /etc/cryptsetup.d
chmod 700 /etc/cryptsetup.d
```

---

## Step 2: Generate LUKS Keyfile

```bash
# Generate 512-byte random keyfile
dd if=/dev/urandom of=/etc/cryptsetup.d/root.key bs=512 count=1

# Set strict permissions
chmod 600 /etc/cryptsetup.d/root.key
```

---

## Step 3: Add Keyfile to LUKS Key Slot

```bash
# Replace /dev/sdX2 with your root partition
cryptsetup luksAddKey /dev/sdX2 /etc/cryptsetup.d/root.key
```

**Enter existing LUKS passphrase** when prompted.

**Verify keyfile was added:**
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

```bash
# Regenerate initramfs (includes keyfile now)
mkinitcpio -P

# Regenerate GRUB config
grub-mkconfig -o /boot/grub/grub.cfg
```

---

## Step 9: Verify Keyfile is in Initramfs

```bash
# List contents of initramfs
lsinitcpio /boot/initramfs-linux.img | grep root.key

# Should show:
# etc/cryptsetup.d/root.key
```

---

**SUCCESS:** Automatic LUKS decryption configured

**Note:** System will now boot without asking for LUKS passphrase (keyfile is in `/boot` partition)

**Next:** Continue with other configuration modules

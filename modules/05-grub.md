# Module: GRUB Bootloader Installation

**Purpose:** Install and configure GRUB bootloader

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)
- EFI partition mounted at `/boot`
- Root partition UUID identified

**Time:** 10-15 minutes

---

## Step 1: Install GRUB

```bash
pacman -S grub efibootmgr os-prober

# Install GRUB to EFI partition
grub-install --target=x86_64-efi \
  --efi-directory=/boot \
  --bootloader-id=GRUB \
  --recheck
```

---

## Step 2: Get Root Partition UUID

```bash
# Display root partition UUID
blkid /dev/sdX2

# Or filter only UUID value
blkid -s UUID -o value /dev/sdX2
```

**Copy the UUID** (format: `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX`)

---

## Step 3: Configure GRUB

```bash
nano /etc/default/grub
```

**For standard installation (no encryption):**
```bash
GRUB_CMDLINE_LINUX="root=UUID=<YOUR_UUID>"
GRUB_DISABLE_OS_PROBER=false
```

**For LUKS encryption:**
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<YOUR_UUID>:cryptroot root=/dev/mapper/cryptroot"
GRUB_DISABLE_OS_PROBER=false
```

**For Btrfs subvolume:**
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<YOUR_UUID>:cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@"
GRUB_DISABLE_OS_PROBER=false
```

**Replace `<YOUR_UUID>` with actual UUID from Step 2**

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

---

## Step 4: Generate GRUB Configuration

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

**Next:** Continue with other configuration modules or exit chroot

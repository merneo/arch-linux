# Module: GRUB Bootloader Installation

**Purpose:** Install and configure GRUB bootloader

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)
- EFI partition mounted at `/boot`
- Root partition UUID identified

**Time:** 10-15 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

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
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<LUKS_UUID>:cryptroot root=/dev/mapper/cryptroot"
GRUB_DISABLE_OS_PROBER=false
```

**For Btrfs subvolume:**
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

# Installation Scenarios

**Purpose:** Guide to choose the right modules based on your installation scenario

---

## Installation Scenarios

### Scenario 1: Dual Boot (Arch Linux + Windows)

**Use this if:** You want to keep Windows and install Arch Linux alongside it.

**Disk Layout:**
- EFI System Partition (shared with Windows)
- Windows partition (NTFS)
- Arch Linux root partition (Btrfs or Btrfs+LUKS)
- Swap partition (optional)

**Required Modules:**
1. `PRE-01-create-arch-usb.md` - Create Arch Linux bootable USB
2. `PRE-02-install-windows.md` - Install Windows first (if not already installed)
3. `06-disk-partitioning.md` - **Use dual boot section** (resize Windows partition or use free space)
2. Choose filesystem:
   - **Option A:** `07-luks-encryption.md` → `08-btrfs-filesystem.md` (Btrfs + LUKS)
   - **Option B:** `08-btrfs-filesystem.md` (Btrfs only, no LUKS)
3. `00-core-installation.md` - Install base system
4. `01-chroot.md` - Enter chroot
5. `02-locale.md` - Set locale
6. `03-root-password.md` - Set root password
7. `04-user-creation.md` - Create user
8. `05-grub.md` - Install GRUB (will detect Windows automatically)
9. `15-exit-chroot.md` - Exit and reboot

**Important Notes:**
- Windows Fast Startup **MUST BE DISABLED** (prevents filesystem corruption)
- BitLocker encryption in Windows **MUST BE DISABLED** before proceeding
- GRUB will automatically detect Windows and add it to boot menu

---

### Scenario 2: Single Boot - Btrfs + LUKS (Full Disk Encryption)

**Use this if:** You want only Arch Linux with full disk encryption.

**Disk Layout:**
- EFI System Partition (512 MB)
- Arch Linux root partition (Btrfs, encrypted with LUKS2)
- Swap partition (encrypted with LUKS2, optional)

**Required Modules:**
1. `PRE-01-create-arch-usb.md` - Create Arch Linux bootable USB
2. `PRE-03-format-disk.md` - Format disk completely (optional, if you want fresh start)
3. `06-disk-partitioning.md` - **Use single boot section** (entire disk for Arch)
2. `07-luks-encryption.md` - Encrypt root and swap partitions
3. `08-btrfs-filesystem.md` - Create Btrfs on encrypted partition
4. `00-core-installation.md` - Install base system
5. `01-chroot.md` - Enter chroot
6. `02-locale.md` - Set locale
7. `03-root-password.md` - Set root password
8. `04-user-creation.md` - Create user
9. `05-grub.md` - Install GRUB (with LUKS parameters)
10. `10-luks-keyfile-auto-unlock.md` - Auto-unlock LUKS (optional)
11. `15-exit-chroot.md` - Exit and reboot

**Security:**
- Full disk encryption (except EFI boot partition)
- Strong LUKS passphrase required
- Optional auto-unlock with keyfile

---

### Scenario 3: Single Boot - Btrfs Only (No Encryption)

**Use this if:** You want only Arch Linux with Btrfs but without encryption.

**Disk Layout:**
- EFI System Partition (512 MB)
- Arch Linux root partition (Btrfs, unencrypted)
- Swap partition (optional)

**Required Modules:**
1. `PRE-01-create-arch-usb.md` - Create Arch Linux bootable USB
2. `PRE-03-format-disk.md` - Format disk completely (optional, if you want fresh start)
3. `06-disk-partitioning.md` - **Use single boot section** (entire disk for Arch)
2. `08-btrfs-filesystem.md` - Create Btrfs (skip LUKS encryption)
3. `00-core-installation.md` - Install base system
4. `01-chroot.md` - Enter chroot
5. `02-locale.md` - Set locale
6. `03-root-password.md` - Set root password
7. `04-user-creation.md` - Create user
8. `05-grub.md` - Install GRUB (standard, no LUKS parameters)
9. `15-exit-chroot.md` - Exit and reboot

**Note:**
- No encryption - data is accessible without passphrase
- Btrfs snapshots still work (for system rollback)
- Faster boot (no encryption overhead)

---

## Quick Decision Tree

**Do you want to keep Windows?**
- **Yes** → Scenario 1: Dual Boot
- **No** → Continue below

**Do you want disk encryption?**
- **Yes** → Scenario 2: Single Boot - Btrfs + LUKS
- **No** → Scenario 3: Single Boot - Btrfs Only

---

## Module Selection by Scenario

| Module | Dual Boot | Single Boot (LUKS) | Single Boot (No LUKS) |
|--------|-----------|-------------------|---------------------|
| `PRE-01-create-arch-usb.md` | ✅ Required | ✅ Required | ✅ Required |
| `PRE-02-install-windows.md` | ✅ Required | ❌ Skip | ❌ Skip |
| `PRE-03-format-disk.md` | ❌ Skip | ⚠️ Optional | ⚠️ Optional |
| `06-disk-partitioning.md` | ✅ (dual boot section) | ✅ (single boot section) | ✅ (single boot section) |
| `07-luks-encryption.md` | ⚠️ Optional | ✅ Required | ❌ Skip |
| `08-btrfs-filesystem.md` | ✅ | ✅ (on encrypted) | ✅ (on unencrypted) |
| `09-mount-partitions.md` | ❌ (use Btrfs) | ❌ (use Btrfs) | ❌ (use Btrfs) |
| `10-luks-keyfile-auto-unlock.md` | ⚠️ Optional | ⚠️ Optional | ❌ Skip |
| `05-grub.md` | ✅ (auto-detect Windows) | ✅ (with LUKS params) | ✅ (standard) |

---

## Detailed Module Instructions

### For Dual Boot Scenario

**Disk Partitioning:**
- Use `06-disk-partitioning.md` → **Dual Boot Section**
- Resize Windows partition (if needed) or use free space
- Create EFI partition (if not exists, Windows usually creates it)
- Create Arch Linux root partition
- Create swap partition (optional)

**Filesystem:**
- Choose: Btrfs + LUKS OR Btrfs only
- If LUKS: Follow `07-luks-encryption.md` → `08-btrfs-filesystem.md`
- If no LUKS: Follow `08-btrfs-filesystem.md` directly

**GRUB:**
- GRUB will automatically detect Windows
- Windows will appear in boot menu

### For Single Boot - Btrfs + LUKS

**Disk Partitioning:**
- Use `06-disk-partitioning.md` → **Single Boot Section**
- Use entire disk for Arch Linux
- Create EFI partition (512 MB)
- Create root partition (rest of disk)
- Create swap partition (optional)

**Encryption:**
- Follow `07-luks-encryption.md` for root and swap
- Use strong passphrase
- Optional: Set up auto-unlock with `10-luks-keyfile-auto-unlock.md`

**Filesystem:**
- Follow `08-btrfs-filesystem.md` on encrypted partition

**GRUB:**
- Use `05-grub.md` with LUKS parameters
- System will prompt for LUKS passphrase on boot

### For Single Boot - Btrfs Only

**Disk Partitioning:**
- Use `06-disk-partitioning.md` → **Single Boot Section**
- Use entire disk for Arch Linux
- Create EFI partition (512 MB)
- Create root partition (rest of disk)
- Create swap partition (optional)

**Filesystem:**
- Follow `08-btrfs-filesystem.md` directly (no encryption)
- Create Btrfs on unencrypted partition

**GRUB:**
- Use `05-grub.md` (standard, no LUKS parameters)
- System boots directly without passphrase

---

**Next:** Choose your scenario and follow the appropriate modules in order.

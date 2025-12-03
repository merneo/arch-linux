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

**Required Phases:**

1. **[Phase 00: Preparation](phases/00-PREPARATION.md)**
   - Create Arch Linux USB
   - Install Windows (if not already installed)
   - Format disk (optional, if fresh install)

2. **[Phase 01: Disk Setup](phases/01-DISK_SETUP.md)**
   - Disk partitioning (dual boot section)
   - Choose filesystem:
     - **Option A:** LUKS encryption → Btrfs (Btrfs + LUKS)
     - **Option B:** Btrfs only (no LUKS)

3. **[Phase 02: System Install](phases/02-SYSTEM_INSTALL.md)**
   - Install base system
   - Enter chroot

4. **[Phase 03: Basic Config](phases/03-BASIC_CONFIG.md)**
   - Set locale
   - Set root password
   - Create user

5. **[Phase 04: Bootloader](phases/04-BOOTLOADER.md)**
   - Install GRUB (will detect Windows automatically)

6. **[Phase 09: Finalize](phases/09-FINALIZE.md)**
   - Exit chroot and reboot

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

**Required Phases:**

1. **[Phase 00: Preparation](phases/00-PREPARATION.md)**
   - Create Arch Linux USB
   - Format disk (optional, if fresh start)

2. **[Phase 01: Disk Setup](phases/01-DISK_SETUP.md)**
   - Disk partitioning (single boot section)
   - LUKS encryption
   - Btrfs filesystem on encrypted partition

3. **[Phase 02: System Install](phases/02-SYSTEM_INSTALL.md)**
   - Install base system
   - Enter chroot

4. **[Phase 03: Basic Config](phases/03-BASIC_CONFIG.md)**
   - Set locale
   - Set root password
   - Create user

5. **[Phase 04: Bootloader](phases/04-BOOTLOADER.md)**
   - Install GRUB (with LUKS parameters)
   - LUKS auto-unlock (optional)

6. **[Phase 09: Finalize](phases/09-FINALIZE.md)**
   - Exit chroot and reboot

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

**Required Phases:**

1. **[Phase 00: Preparation](phases/00-PREPARATION.md)**
   - Create Arch Linux USB
   - Format disk (optional, if fresh start)

2. **[Phase 01: Disk Setup](phases/01-DISK_SETUP.md)**
   - Disk partitioning (single boot section)
   - Btrfs filesystem (skip LUKS encryption)

3. **[Phase 02: System Install](phases/02-SYSTEM_INSTALL.md)**
   - Install base system
   - Enter chroot

4. **[Phase 03: Basic Config](phases/03-BASIC_CONFIG.md)**
   - Set locale
   - Set root password
   - Create user

5. **[Phase 04: Bootloader](phases/04-BOOTLOADER.md)**
   - Install GRUB (standard, no LUKS parameters)

6. **[Phase 09: Finalize](phases/09-FINALIZE.md)**
   - Exit chroot and reboot

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

## Phase Selection by Scenario

| Phase | Dual Boot | Single Boot (LUKS) | Single Boot (No LUKS) |
|------|-----------|-------------------|---------------------|
| **00-PREPARATION** | ✅ Required | ✅ Required | ✅ Required |
| - PRE-01-create-arch-usb | ✅ Required | ✅ Required | ✅ Required |
| - PRE-02-install-windows | ✅ Required | ❌ Skip | ❌ Skip |
| - PRE-03-format-disk | ❌ Skip | ⚠️ Optional | ⚠️ Optional |
| **01-DISK_SETUP** | ✅ Required | ✅ Required | ✅ Required |
| - 06-disk-partitioning | ✅ (dual boot) | ✅ (single boot) | ✅ (single boot) |
| - 07-luks-encryption | ⚠️ Optional | ✅ Required | ❌ Skip |
| - 08-btrfs-filesystem | ✅ | ✅ (on encrypted) | ✅ (on unencrypted) |
| **02-SYSTEM_INSTALL** | ✅ Required | ✅ Required | ✅ Required |
| **03-BASIC_CONFIG** | ✅ Required | ✅ Required | ✅ Required |
| **04-BOOTLOADER** | ✅ Required | ✅ Required | ✅ Required |
| - 05-grub | ✅ (auto-detect Windows) | ✅ (with LUKS) | ✅ (standard) |
| - 10-luks-keyfile-auto-unlock | ⚠️ Optional | ⚠️ Optional | ❌ Skip |
| **05-NETWORK** | ⚠️ Optional | ⚠️ Optional | ⚠️ Optional |
| **06-AUDIO** | ⚠️ Optional | ⚠️ Optional | ⚠️ Optional |
| **07-SECURITY** | ⚠️ Optional | ⚠️ Optional | ⚠️ Optional |
| **08-LAPTOP_HARDWARE** | ⚠️ Optional (laptops) | ⚠️ Optional (laptops) | ⚠️ Optional (laptops) |
| **09-FINALIZE** | ✅ Required | ✅ Required | ✅ Required |

---

## Detailed Module Instructions

### For Dual Boot Scenario

**Follow phases in order:**
1. **[Phase 00: Preparation](phases/00-PREPARATION.md)** - Create USB, install Windows
2. **[Phase 01: Disk Setup](phases/01-DISK_SETUP.md)** - Partition (dual boot section), choose filesystem
3. **[Phase 02: System Install](phases/02-SYSTEM_INSTALL.md)** - Install base system
4. **[Phase 03: Basic Config](phases/03-BASIC_CONFIG.md)** - Locale, user, root password
5. **[Phase 04: Bootloader](phases/04-BOOTLOADER.md)** - GRUB (auto-detects Windows)
6. **[Phase 09: Finalize](phases/09-FINALIZE.md)** - Exit and reboot

**Note:** GRUB will automatically detect Windows and add it to boot menu

### For Single Boot - Btrfs + LUKS

**Follow phases in order:**
1. **[Phase 00: Preparation](phases/00-PREPARATION.md)** - Create USB, format disk (optional)
2. **[Phase 01: Disk Setup](phases/01-DISK_SETUP.md)** - Partition (single boot), LUKS encryption, Btrfs
3. **[Phase 02: System Install](phases/02-SYSTEM_INSTALL.md)** - Install base system
4. **[Phase 03: Basic Config](phases/03-BASIC_CONFIG.md)** - Locale, user, root password
5. **[Phase 04: Bootloader](phases/04-BOOTLOADER.md)** - GRUB (with LUKS), auto-unlock (optional)
6. **[Phase 09: Finalize](phases/09-FINALIZE.md)** - Exit and reboot

**Note:** System will prompt for LUKS passphrase on boot (unless auto-unlock configured)

### For Single Boot - Btrfs Only

**Follow phases in order:**
1. **[Phase 00: Preparation](phases/00-PREPARATION.md)** - Create USB, format disk (optional)
2. **[Phase 01: Disk Setup](phases/01-DISK_SETUP.md)** - Partition (single boot), Btrfs (no LUKS)
3. **[Phase 02: System Install](phases/02-SYSTEM_INSTALL.md)** - Install base system
4. **[Phase 03: Basic Config](phases/03-BASIC_CONFIG.md)** - Locale, user, root password
5. **[Phase 04: Bootloader](phases/04-BOOTLOADER.md)** - GRUB (standard, no LUKS)
6. **[Phase 09: Finalize](phases/09-FINALIZE.md)** - Exit and reboot

**Note:** System boots directly without passphrase

---

**Next:** Choose your scenario and follow the appropriate [phases](phases/) in order.

**For detailed step-by-step instructions:** See individual [steps](steps/) within each phase.

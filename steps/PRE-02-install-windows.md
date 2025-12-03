# Module: Install Windows (Dual Boot)

**Purpose:** Install Windows on disk before installing Arch Linux (for dual boot scenario)

**Prerequisites:**
- Bootable Windows installation USB (created with Windows Media Creation Tool)
- Target computer ready for Windows installation
- BIOS configured for UEFI boot mode

**Time:** 60-90 minutes (includes Windows installation + updates)

**ENVIRONMENT:** Windows installation environment

**Note:** This module is **ONLY for dual boot scenario**. If you want only Arch Linux, skip this module and go directly to `00-core-installation.md`.

**⚠️ IMPORTANT:** This module must be completed **BEFORE** installing Arch Linux. Windows must be installed first for dual boot.

---

## ⚠️ WARNING: Data Loss

**This module will PERMANENTLY DELETE all data on the target disk.**

- Make sure you have backups of important data
- Double-check disk selection during Windows installation
- This operation CANNOT be undone
- Verify you're working with the correct disk - **wrong disk selection will destroy data**

---

## Step 1: Prepare BIOS Settings

**Before installing Windows:** For a deeper understanding of these settings, refer to the [ArchWiki on Unified Extensible Firmware Interface (UEFI)](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface) and [ArchWiki on Secure Boot](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface#Secure_Boot).

1. **Power on target computer**
2. **Enter BIOS Setup:**
   - Press BIOS key repeatedly during boot (F2, F10, DEL, ESC - depends on manufacturer)
   - Common keys: **F10** (HP), **F2** (Dell), **DEL** (ASUS)

3. **Configure Boot Settings:**
   - **Boot Mode**: Set to **UEFI** (not Legacy/CSM)
   - **Secure Boot**: **Disable** (temporarily, can re-enable later, refer to [ArchWiki: Secure Boot](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface#Secure_Boot))
   - **Fast Boot**: **Disable** (if available, prevents proper shutdown, refer to [ArchWiki: Dual boot with Windows#Disable Fast Startup](https://wiki.archlinux.org/title/Dual_boot_with_Windows#Disable_Fast_Startup))

4. **Save and Exit** (usually F10 → Yes)

---

## Step 2: Create Windows Installation USB

**On a Windows computer (can be the target computer if Windows is already installed):**

### Option A: Windows Media Creation Tool (Recommended)

1. **Download Windows Media Creation Tool:**
   - Visit: https://www.microsoft.com/software-download/windows10 (for Windows 10)
   - Or: https://www.microsoft.com/software-download/windows11 (for Windows 11)
   - Click **"Download tool now"**
   - Save `MediaCreationTool.exe` to desktop

2. **Run Media Creation Tool:**
   - Right-click `MediaCreationTool.exe` → **Run as administrator**
   - Accept license terms
   - Select **"Create installation media (USB flash drive, DVD, or ISO file)"**
   - Click **Next**

3. **Configure Installation Media:**
   - **Language**: Select your language (e.g., English)
   - **Edition**: Select **Windows 10/11** (Home or Pro, matching your license)
   - **Architecture**: Select **64-bit (x64)**
   - Click **Next**

4. **Select Media Type:**
   - Choose **"USB flash drive"**
   - Insert **USB flash drive** (8 GB minimum, will be formatted)
   - Select the USB drive from the list
   - Click **Next**

5. **Media Creation Process:**
   - Tool downloads Windows ISO (~4-5 GB)
   - Tool creates bootable USB with Windows installation files
   - Process takes 15-30 minutes depending on internet speed
   - Click **Finish** when complete

### Option B: Download ISO and Use Rufus

1. **Download Windows ISO:**
   - Visit: https://www.microsoft.com/software-download/windows10 or Windows 11
   - Download ISO file directly

2. **Use Rufus to create bootable USB:**
   - Download Rufus: https://rufus.ie/
   - Open Rufus
   - **Device**: Select USB drive
   - **Boot selection**: Click "SELECT" → Choose Windows ISO
   - **Partition scheme**: GPT
   - **Target system**: UEFI (non CSM)
   - Click **START**

---

## Step 3: Boot from Windows Installation USB

1. **Insert Windows installation USB** into target computer
2. **Power on computer**
3. **Enter Boot Menu:**
   - Press boot menu key during boot (F9, F12, ESC, or DEL - depends on manufacturer)
   - Common keys: **F9** (HP), **F12** (Dell), **ESC** (ASUS)
4. **Select USB drive** from boot menu
5. **Windows Setup should load** (~30 seconds)

---

## Step 4: Windows Installation Wizard

1. **Language, Time, Keyboard:**
   - Select your preferences
   - Click **Next**

2. **Click "Install Now"**

3. **Product Key:**
   - Enter Windows product key OR
   - Select **"I don't have a product key"** (activate later)
   - Click **Next**

4. **Select Windows Edition:**
   - Choose: Home, Pro, or Enterprise (matching your license)
   - Click **Next**

5. **Accept License Terms:**
   - Check the box
   - Click **Next**

6. **Installation Type:**
   - Select **"Custom: Install Windows only (advanced)"**

---

## Step 5: Partition Disk for Dual Boot

**⚠️ CRITICAL:** This step determines dual-boot success. You **MUST** leave free space for Arch Linux.

### Option A: Fresh Install (Empty Disk)

**If disk is empty or you want to delete everything:**

1. **Delete all existing partitions** (if any):
   - Select each partition
   - Click **Delete**
   - Confirm deletion
   - Repeat for all partitions

2. **Create EFI System Partition:**
   - Select **Unallocated space**
   - Click **New**
   - **Size**: `512` MB (minimum for EFI)
   - Click **Apply**
   - Windows will create additional small partitions (System Reserved) - this is normal

3. **Create Windows Partition:**
   - Select remaining **Unallocated space**
   - Click **New**
   - **Size**: Leave **~50-70% of disk** for Windows (e.g., `150G` or `200G`)
   - **IMPORTANT:** Do NOT use entire disk - leave free space for Arch Linux!
   - Click **Apply**

4. **Verify Layout:**
   - Should show:
     - **System Reserved** (small, ~100-500 MB)
     - **EFI System Partition** (512 MB)
     - **Windows (C:)** (your chosen size, e.g., 150 GB)
     - **Unallocated space** (~30-50% of disk) ← **This is for Arch Linux!**

5. **Select Windows partition** (the large one, C:)
6. **Click Next** to install Windows

### Option B: Existing Windows (Resize)

**If Windows is already installed and you want to resize:**

1. **Boot into Windows**
2. **Open Disk Management:**
   - Press `Win + X`
   - Select **"Disk Management"**

3. **Shrink Windows Partition:**
   - Right-click Windows partition (C:)
   - Select **"Shrink Volume"**
   - Enter amount to shrink (e.g., `70000` MB = 70 GB for Arch Linux)
   - Click **Shrink**

4. **Verify:**
   - Should show **Unallocated space** after Windows partition
   - This space will be used for Arch Linux

---

## Step 6: Complete Windows Installation

1. **Windows installation process:**
   - Windows will install (20-40 minutes)
   - Computer will restart automatically several times
   - **Do not remove USB** until installation is complete

2. **Initial Setup:**
   - Follow Windows setup wizard
   - Create user account
   - Configure privacy settings
   - Wait for Windows to finish setup

3. **Windows Updates:**
   - Windows will download and install updates
   - This may take 30-60 minutes
   - Computer may restart several times

---

## Step 7: Configure Windows for Dual Boot

**⚠️ CRITICAL:** These settings **MUST** be configured before installing Arch Linux. For more detailed explanations, refer to the [ArchWiki on Dual boot with Windows](https://wiki.archlinux.org/title/Dual_boot_with_Windows).

### Disable Fast Startup

1. **Open Control Panel:**
   - Press `Win + X`
   - Select **"Control Panel"**

2. **Power Options:**
   - Click **"Power Options"**
   - Click **"Choose what the power buttons do"** (left sidebar)

3. **Change settings that are currently unavailable:**
   - Click **"Change settings that are currently unavailable"** (requires admin)
   - Enter admin password if prompted

4. **Uncheck "Turn on fast startup":**
   - Find **"Turn on fast startup (recommended)"**
   - **Uncheck** the box
   - Click **"Save changes"**

**Why:** Fast Startup prevents proper shutdown and can cause filesystem corruption when dual booting. See [ArchWiki: Dual boot with Windows#Disable Fast Startup](https://wiki.archlinux.org/title/Dual_boot_with_Windows#Disable_Fast_Startup) for more information.

### Disable BitLocker (if enabled)

1. **Open BitLocker settings:**
   - Press `Win + X`
   - Select **"Control Panel"**
   - Click **"BitLocker Drive Encryption"**

2. **Disable BitLocker:**
   - Find your Windows drive (C:)
   - Click **"Turn off BitLocker"**
   - Confirm decryption
   - Wait for decryption to complete (may take hours if disk is large)

**Why:** BitLocker encryption can interfere with Arch Linux installation and GRUB. Refer to [ArchWiki: Dual boot with Windows#Disable BitLocker](https://wiki.archlinux.org/title/Dual_boot_with_Windows#Disable_BitLocker) for further context.

### Verify EFI System Partition

1. **Open Disk Management:**
   - Press `Win + X`
   - Select **"Disk Management"**

2. **Verify EFI partition exists:**
   - Should see **EFI System Partition** (512 MB, FAT32)
   - This partition will be shared with Arch Linux. For details on EFI system partition, refer to [ArchWiki: EFI system partition](https://wiki.archlinux.org/title/EFI_system_partition).

---

## Step 8: Shut Down Windows Completely

**Before installing Arch Linux:**

1. **Click Start** → **Power** → **Shut down** (NOT Restart)
2. **Wait for system to power off completely**
3. **Remove Windows installation USB** (if still inserted)

**⚠️ IMPORTANT:** Use **Shut down**, not Restart. This ensures Windows releases all disk locks.

---

## Verification

**Before proceeding to Arch Linux installation, verify:**

- [ ] Windows is installed and boots successfully
- [ ] Fast Startup is **DISABLED**
- [ ] BitLocker is **DISABLED** (if it was enabled)
- [ ] EFI System Partition exists (512 MB, FAT32)
- [ ] Free space exists on disk for Arch Linux (check with Disk Management)
- [ ] Windows shuts down completely (not hibernated)

---

## Troubleshooting

### Problem: Windows installation fails
**Solution:**
1. Verify UEFI boot mode is enabled
2. Disable Secure Boot (temporarily)
3. Try different USB port
4. Verify Windows ISO is not corrupted

### Problem: No free space for Arch Linux
**Solution:**
1. Boot into Windows
2. Use Disk Management to shrink Windows partition
3. Leave at least 30-50 GB for Arch Linux

### Problem: Fast Startup cannot be disabled
**Solution:**
1. Run Control Panel as administrator
2. Verify you have admin privileges
3. Try disabling from Group Policy Editor (advanced)

---

**SUCCESS:** Windows installed and configured for dual boot

**Next:**
- `PRE-01-create-arch-usb.md` - Create Arch Linux USB (if not done yet)
- `00-core-installation.md` - Boot from Arch Linux USB and start installation

**Official Resources:**
- [Windows 10 Download](https://www.microsoft.com/software-download/windows10)
- [Windows 11 Download](https://www.microsoft.com/software-download/windows11)
- [ArchWiki: Dual Boot with Windows](https://wiki.archlinux.org/title/Dual_boot_with_Windows)

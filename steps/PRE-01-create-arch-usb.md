# Module: Create Arch Linux Bootable USB

**Purpose:** Create bootable USB drive with Arch Linux Live ISO. For detailed instructions and troubleshooting, refer to the [ArchWiki on USB flash installation media](https://wiki.archlinux.org/title/USB_flash_installation_media).

**Prerequisites:**
- Second computer (preparation machine) with internet connection
- USB flash drive (8 GB minimum, will be formatted - all data will be lost)
- Arch Linux ISO file (will be downloaded)

**Time:** 10-20 minutes (depending on download speed)

**ENVIRONMENT:** Preparation machine (Linux, Windows, or macOS)

**Note:** This module must be completed **before** you can boot from Arch Linux Live USB. You need a second computer to create the USB drive.

---

## ⚠️ WARNING: Data Loss

**This module will PERMANENTLY DELETE all data on the USB drive.**

- Make sure you have backups of important data on USB drive
- Double-check USB device name before proceeding (use `lsblk` or `diskutil list`)
- This operation CANNOT be undone
- Verify you're working with the correct USB drive - **wrong device selection will destroy data**

---

## Step 1: Download Arch Linux ISO

**On your preparation machine (second computer):**

### Linux/macOS:

```bash
# Download latest Arch Linux ISO. See [Arch Linux Download](https://archlinux.org/download/).
wget https://mirror.rackspace.com/archlinux/iso/latest/archlinux-x86_64.iso

# Download signature for verification (optional but recommended)
wget https://mirror.rackspace.com/archlinux/iso/latest/archlinux-x86_64.iso.sig

# Verify ISO integrity (optional but recommended), using GnuPG.
# For details, refer to [ArchWiki: Installation guide#Verify signature](https://wiki.archlinux.org/title/Installation_guide#Verify_signature).
gpg --verify archlinux-x86_64.iso.sig archlinux-x86_64.iso
```

**Expected output:**
```
gpg: Good signature from "Pierre Schmitz <pierre@archlinux.org>"
```

### Windows:

1. Visit: https://archlinux.org/download/
2. Select a mirror near you
3. Download: `archlinux-YYYY.MM.DD-x86_64.iso` (latest version)
4. Save to desktop or Downloads folder

**File size:** ~900 MB - 1 GB

---

## Step 2: Identify USB Device

**⚠️ CRITICAL:** Identify your USB drive correctly. Writing to wrong device will destroy data!

### Linux:

```bash
# List all block devices. For more details on lsblk, refer to the [ArchWiki on lsblk](https://wiki.archlinux.org/title/Lsblk).
lsblk

# Look for your USB drive (e.g., /dev/sdb, NOT /dev/sdb1)
# USB drives are typically smaller (8-32 GB)
# Note the device name (e.g., /dev/sdb)
```

**Example output:**
```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 500.1G  0 disk
├─sda1   8:1    0   512M  0 part /boot
└─sda2   8:2    0 499.6G  0 part /
sdb      8:16   1  14.9G  0 disk    ← This is your USB drive
└─sdb1   8:17   1  14.9G  0 part
```

In this example, USB drive is `/dev/sdb` (NOT `/dev/sdb1`).

### Windows:

1. Insert USB drive
2. Open File Explorer
3. Note the drive letter (e.g., `E:`, `F:`)
4. Open Rufus (see Step 3) - it will show USB drives

### macOS:

```bash
# List all disks. For more information on diskutil, refer to [Apple's diskutil man page](https://ss64.com/osx/diskutil.html).
diskutil list

# Look for your USB drive (e.g., /dev/disk2)
# USB drives are typically smaller (8-32 GB)
```

**Example output:**
```
/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *15.0 GB    disk2
   1:                 DOS_FAT_32 USB                    15.0 GB     disk2s1
```

In this example, USB drive is `/dev/disk2` (NOT `/dev/disk2s1`).

---

## Step 3: Create Bootable USB

### Linux:

```bash
# Unmount USB drive if mounted
sudo umount /dev/sdX1

# Write ISO to USB (**WARNING:** REPLACE /dev/sdX WITH YOUR USB DEVICE)
# For detailed information on using 'dd' for creating installation media,
# refer to the [ArchWiki on USB flash installation media](https://wiki.archlinux.org/title/USB_flash_installation_media#Using_dd).
# The 'bs=4M' sets the block size to 4 megabytes for faster transfer,
# and 'oflag=sync' ensures data is written synchronously.
# Consult 'man dd' for more options.
# Replace /dev/sdX with your actual USB device (e.g., /dev/sdb)
sudo dd if=archlinux-x86_64.iso of=/dev/sdX bs=4M status=progress oflag=sync

# Wait for completion (2-5 minutes)
sync
```

**Example:**
```bash
sudo dd if=archlinux-x86_64.iso of=/dev/sdb bs=4M status=progress oflag=sync
```

**Expected output:**
```
237+1 records in
237+1 records out
995098624 bytes (995 MB, 949 MiB) copied, 45.2345 s, 22.0 MB/s
```

### Windows (using Rufus):

1. **Download Rufus:**
   - Visit: [https://rufus.ie/](https://rufus.ie/)
   - Download latest version (portable or installer)

2. **Insert USB drive** (8 GB minimum)

3. **Open Rufus** (run as administrator)

4. **Configure Rufus:**
   - **Device**: Select your USB drive from dropdown
   - **Boot selection**: Click **"SELECT"** button
   - Choose `archlinux-YYYY.MM.DD-x86_64.iso` file
   - **Partition scheme**: **GPT**
   - **Target system**: **UEFI (non CSM)**
   - **File system**: **FAT32** (default)
   - **Volume label**: Leave default or set to "ARCH_202501"

5. **Click "START"**

6. **Write mode dialog:**
   - Select **"DD Image"** (important!)
   - Click **OK** to confirm

7. **Warning dialog:**
   - "All data on device will be destroyed"
   - Click **OK** to proceed

8. **Wait for completion** (3-5 minutes)

9. **Success message:**
   - "READY" status appears
   - Click **CLOSE**

### macOS:

```bash
# Unmount USB drive
diskutil unmountDisk /dev/diskX

# Write ISO to USB (**WARNING:** REPLACE /dev/diskX WITH YOUR USB DEVICE)
# Use /dev/rdiskX (raw disk) for faster writing
sudo dd if=archlinux-x86_64.iso of=/dev/rdiskX bs=4m && sync
```

**Example:**
```bash
diskutil unmountDisk /dev/disk2
sudo dd if=archlinux-x86_64.iso of=/dev/rdisk2 bs=4m && sync
```

**Wait for completion** (2-5 minutes)

---

## Step 4: Verify USB Creation

### Linux:

```bash
# Check USB device
lsblk /dev/sdX

# Should show partitions created by ISO
```

### Windows:

1. Open File Explorer
2. Check USB drive
3. Should show Arch Linux files and folders

### macOS:

```bash
# Check USB device
diskutil list /dev/diskX

# Should show partitions created by ISO
```

---

## Step 5: Test Boot (Optional but Recommended)

**Before proceeding to installation:**

1. **Insert USB drive** into target computer
2. **Boot from USB:**
   - Power on computer
   - Press boot menu key (F9, F12, ESC, or DEL - depends on manufacturer)
   - Select USB drive from boot menu
3. **Verify Arch Linux Live environment loads:**
   - Should see Arch Linux boot menu
   - Should boot into Arch Linux live environment
   - Should see `root@archiso` prompt

**If boot fails:**
- Verify [UEFI boot mode](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface) is enabled in BIOS
- Try disabling [Secure Boot](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface#Secure_Boot) (temporarily)
- Try recreating USB with different tool
- Verify ISO download is complete and not corrupted

---

## Troubleshooting

### Problem: USB not booting
**Solution:**
1. Verify UEFI boot mode is enabled in BIOS
2. Disable Secure Boot (temporarily)
3. Try recreating USB with different tool
4. Verify ISO file is not corrupted

### Problem: Wrong device selected
**Solution:**
1. **STOP immediately** if you realize you selected wrong device
2. Data may already be lost, but stop to prevent further damage
3. Use `lsblk` or `diskutil list` to carefully identify USB device
4. Double-check device name before running `dd` command

### Problem: "Permission denied" on Linux
**Solution:**
1. Use `sudo` before `dd` command
2. Verify you have sudo privileges

### Problem: USB creation fails on Windows
**Solution:**
1. Run Rufus as administrator
2. Try different USB port
3. Try different USB drive
4. Verify ISO file is not corrupted

---

**SUCCESS:** Arch Linux bootable USB created successfully

**Next:**
- **For dual boot:** `PRE-02-install-windows.md` - Install Windows first
- **For single boot:** `core-installation.md` - Boot from USB and start installation

**Official Resources:**
- [Arch Linux Download](https://archlinux.org/download/)
- [ArchWiki: USB Installation Media](https://wiki.archlinux.org/title/USB_flash_installation_media)
- [Rufus](https://rufus.ie/) - Windows USB creation tool

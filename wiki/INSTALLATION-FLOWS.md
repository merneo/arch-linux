# Installation Flows

**Purpose:** Pre-built installation procedures for common scenarios

Choose the flow that matches your situation:

- **[Minimal Installation](#minimal-installation)** - Just system + root password
- **[Standard Installation](#standard-installation)** - System + user + network
- **[Full Installation](#full-installation)** - LUKS + Btrfs + all features

---

## Minimal Installation

**Use this if:** You want the absolute minimum - just install the system and set root password.

**Time:** 35-55 minutes (includes USB preparation)

### Steps:

1. **[Create Arch Linux USB](../modules/PRE-01-create-arch-usb.md)** - Create bootable USB drive
2. **Boot from USB** and connect to network
3. **[Core Installation](02-CORE-INSTALLATION.md)** - Install base system, enter chroot, set root password
4. **[GRUB Bootloader](03-POST-INSTALLATION.md#grub-bootloader)** - Install GRUB (required for boot)
5. **[Exit & Reboot](03-POST-INSTALLATION.md#exit-reboot)** - Final step

**Note:** Assumes disk is already partitioned and mounted at `/mnt`

---

## Standard Installation

**Use this if:** You want a working system with user account and network.

**Time:** 40-60 minutes (includes USB preparation)

### Steps:

1. **[Create Arch Linux USB](../modules/PRE-01-create-arch-usb.md)** - Create bootable USB drive
2. **Boot from USB** and connect to network
3. **[Pre-Installation](01-PRE-INSTALLATION.md)** (if needed)
   - Disk Partitioning (if disk not already partitioned)
   - Mount Partitions

4. **[Core Installation](02-CORE-INSTALLATION.md)**
   - Install base system
   - Enter chroot
   - Set root password

5. **[Post-Installation](03-POST-INSTALLATION.md)**
   - Locale & Timezone
   - User Creation
   - GRUB Bootloader
   - NetworkManager
   - Exit & Reboot

---

## Full Installation

**Use this if:** You want everything - LUKS encryption, Btrfs, and all features.

**Time:** 1.5-2.5 hours (includes USB preparation)

### Steps:

1. **[Create Arch Linux USB](../modules/PRE-01-create-arch-usb.md)** - Create bootable USB drive
2. **[Format Disk](../modules/PRE-03-format-disk.md)** - Completely wipe disk (optional, for fresh install)
3. **Boot from USB** and connect to network
4. **[Pre-Installation](01-PRE-INSTALLATION.md)**
   - Disk Partitioning
   - LUKS Encryption
   - Btrfs Filesystem

5. **[Core Installation](02-CORE-INSTALLATION.md)**
   - Install base system
   - Enter chroot
   - Set root password

6. **[Post-Installation](03-POST-INSTALLATION.md)**
   - Locale & Timezone
   - User Creation
   - GRUB Bootloader (with LUKS parameters)
   - LUKS Auto-Unlock (optional)
   - NetworkManager
   - WiFi Support
   - Bluetooth
   - Audio Server
   - Exit & Reboot

---

## Custom Installation

**Use this if:** None of the pre-built flows match your needs.

1. **Read [Module Index](../MODULE_INDEX.md)** to see all available modules
2. **Read [Generate Procedure](../GENERATE_PROCEDURE.md)** to build your custom procedure
3. **Pick modules** you need
4. **Follow modules in order** (respecting prerequisites)

---

**Back to:** [Home](00-HOME.md) | **Complete Guide:** [COMPLETE-INSTALLATION.md](COMPLETE-INSTALLATION.md)

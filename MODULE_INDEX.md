# Module Index - Quick Reference

**Purpose:** Quick reference to find the right modules for your installation

---

## Core Installation (Required)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `00-core-installation.md` | Install base Arch Linux system | **Always** - This is the core installation |
| `01-chroot.md` | Enter chroot environment | **Always** - Required for all configuration |

---

## Basic Configuration (Recommended)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `02-locale.md` | Set locale and timezone | **Recommended** - Set your language and timezone |
| `03-root-password.md` | Set root password | **Required** - Set root password for system recovery |
| `04-user-creation.md` | Create user account | **Recommended** - Create non-root user with sudo |

---

## Bootloader (Required)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `05-grub.md` | Install GRUB bootloader | **Required** - System won't boot without this |

---

## Disk and Filesystem (Choose One)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `06-disk-partitioning.md` | Create disk partitions | **If needed** - Only if disk is not already partitioned |
| `07-luks-encryption.md` | Encrypt partitions | **Optional** - If you want disk encryption |
| `08-btrfs-filesystem.md` | Create Btrfs with subvolumes | **Optional** - If you want Btrfs filesystem |
| `09-mount-partitions.md` | Mount partitions (non-Btrfs) | **If needed** - If using ext4/xfs instead of Btrfs |

---

## Advanced Configuration (Optional)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `10-luks-keyfile-auto-unlock.md` | Auto-unlock LUKS on boot | **Optional** - If you want automatic decryption |
| `11-networkmanager.md` | Enable NetworkManager | **Recommended** - For network connectivity |
| `12-wifi.md` | WiFi support | **If needed** - If you use WiFi |
| `13-bluetooth.md` | Bluetooth support | **If needed** - If you have Bluetooth devices |
| `14-audio.md` | Audio server (PipeWire) | **If needed** - If you need audio |

---

## Final Steps (Required)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `15-exit-chroot.md` | Exit chroot and reboot | **Always** - Final step before first boot |

---

## Quick Installation Flows

### Minimal Installation (Just System + Root Password)

1. `00-core-installation.md` - Install base system
2. `01-chroot.md` - Enter chroot
3. `03-root-password.md` - Set root password
4. `05-grub.md` - Install GRUB
5. `15-exit-chroot.md` - Exit and reboot

### Standard Installation (System + User + Network)

1. `00-core-installation.md` - Install base system
2. `01-chroot.md` - Enter chroot
3. `02-locale.md` - Set locale
4. `03-root-password.md` - Set root password
5. `04-user-creation.md` - Create user
6. `05-grub.md` - Install GRUB
7. `11-networkmanager.md` - Enable NetworkManager
8. `15-exit-chroot.md` - Exit and reboot

### Full Installation (LUKS + Btrfs + All Features)

1. `06-disk-partitioning.md` - Create partitions (if needed)
2. `07-luks-encryption.md` - Encrypt partitions
3. `08-btrfs-filesystem.md` - Create Btrfs
4. `00-core-installation.md` - Install base system
5. `01-chroot.md` - Enter chroot
6. `02-locale.md` - Set locale
7. `03-root-password.md` - Set root password
8. `04-user-creation.md` - Create user
9. `05-grub.md` - Install GRUB (with LUKS parameters)
10. `10-luks-keyfile-auto-unlock.md` - Auto-unlock (optional)
11. `11-networkmanager.md` - Enable NetworkManager
12. `12-wifi.md` - WiFi support
13. `13-bluetooth.md` - Bluetooth
14. `14-audio.md` - Audio server
15. `15-exit-chroot.md` - Exit and reboot

---

## Module Dependencies

Each module lists its prerequisites. Common dependencies:

- **Most modules** require `01-chroot.md` (you must be in chroot)
- **GRUB** (`05-grub.md`) requires partitions to be mounted
- **LUKS modules** (`07-luks-encryption.md`, `10-luks-keyfile-auto-unlock.md`) require `06-disk-partitioning.md`
- **Btrfs** (`08-btrfs-filesystem.md`) can work with or without LUKS
- **Network modules** (`11-networkmanager.md`, `12-wifi.md`) require `00-core-installation.md`

---

## How to Use

1. **Read this index** to understand available modules
2. **Choose modules** you need for your installation
3. **Check prerequisites** in each module
4. **Follow modules in order** (respecting dependencies)
5. **Verify success** after each module

---

**Remember:** This system assumes you're already booted from Arch Linux Live USB. It doesn't format disks or create USB drives - it only installs and configures the system.

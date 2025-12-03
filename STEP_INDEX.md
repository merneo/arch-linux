# Step Index - Individual Command Reference

**Purpose:** This index provides a detailed, granular list of individual installation steps (usually single commands or small sets of commands) that make up the modules. It's designed for quick reference, debugging, or for experienced users building highly customized installation procedures from scratch.

**Recommendation:** For a structured installation, it's highly recommended to follow the **[Installation Flows](wiki/INSTALLATION-FLOWS.md)** or **[Phases](phases/PREPARATION.md)** instead of picking individual steps directly from here.

---

## Installation Phases (Overview)

For an organized installation, you can follow the structured phases. Each phase links to a detailed guide.

1.  **[Phase 00: Preparation](phases/PREPARATION.md)** - USB creation, Windows installation, disk formatting
2.  **[Phase 01: Disk Setup](phases/DISK_SETUP.md)** - Partitioning, encryption, filesystem
3.  **[Phase 02: System Install](phases/SYSTEM_INSTALL.md)** - Base system installation, chroot
4.  **[Phase 03: Basic Config](phases/BASIC_CONFIG.md)** - Locale, user creation, sudo
5.  **[Phase 04: Bootloader](phases/BOOTLOADER.md)** - GRUB installation, LUKS auto-unlock
6.  **[Phase 05: Network](phases/NETWORK.md)** - NetworkManager, WiFi, Bluetooth
7.  **[Phase 06: Audio](phases/AUDIO.md)** - PipeWire audio
8.  **[Phase 07: Security](phases/SECURITY.md)** - SSH, UFW, fail2ban
9.  **[Phase 08: Hardware](phases/HARDWARE.md)** - Touchpad, webcam, IR camera, fingerprint
10. **[Phase 09: Finalize](phases/FINALIZE.md)** - Exit chroot, reboot

---

## Preparation Steps (Pre-Installation)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `PRE-01-create-arch-usb.md` | Create bootable Arch Linux USB drive | **Always** - Required before installation |
| `PRE-02-install-windows.md` | Install Windows for dual boot | **Dual boot only** - Install Windows before Arch Linux |
| `PRE-03-format-disk.md` | Completely wipe and format disk | **Fresh install only** - If you want to wipe entire disk |

**Note:**
- `PRE-01` is required for all installations.
- `PRE-02` is only for dual boot scenarios.
- `PRE-03` is optional (only if you want to completely wipe disk).

---

## Core Installation Steps (Required)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `core-installation.md` | Install base Arch Linux system | **Always** - This is the core installation. |
| `chroot.md` | Enter chroot environment | **Always** - Required for all configuration steps inside the new system. |

---

## Basic System Configuration (Recommended)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `locale.md` | Set system locale and timezone | **Recommended** - Set your language and timezone. |
| `user-creation.md` | Create user account | **Recommended** - Create non-root user with sudo access. |

---

## Bootloader (Required)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `grub.md` | Install GRUB bootloader | **Required** - System won't boot without this. |

---

## Disk and Filesystem Management (Choose One or More)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `disk-partitioning.md` | Create disk partitions | **If needed** - Only if disk is not already partitioned. |
| `luks-encryption.md` | Encrypt partitions | **Optional** - If you want full disk encryption. |
| `btrfs-filesystem.md` | Create Btrfs with subvolumes | **Optional** - If you want to use Btrfs filesystem. |
| `mount-partitions.md` | Mount partitions (non-Btrfs) | **If needed** - If using Ext4/XFS instead of Btrfs. |

---

## Advanced System Configuration (Optional)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `luks-keyfile-auto-unlock.md` | Auto-unlock LUKS on boot | **Optional** - If you want automatic decryption at boot. |
| `networkmanager.md` | Enable NetworkManager | **Recommended** - For managing network connectivity. |
| `wifi.md` | WiFi support | **If needed** - If your system uses WiFi. |
| `bluetooth.md` | Bluetooth support | **If needed** - If your system has Bluetooth devices. |
| `audio.md` | Audio server (PipeWire) | **If needed** - If you require sound. |

---

## Security and Network Services (Optional)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `ssh-server.md` | Configure SSH server | **If needed** - For remote SSH access. |
| `ufw-firewall.md` | Configure UFW firewall | **Recommended** - For system protection. |
| `fail2ban.md` | Configure fail2ban for SSH | **Recommended** - To protect SSH from brute-force attacks. |

---

## Hardware Integration (Laptops Only)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `touchpad.md` | Configure touchpad/trackpad | **Laptops only** - For touchpad functionality. |
| `webcam.md` | Configure built-in webcam | **Laptops only** - For webcam functionality. |
| `ir-camera.md` | Configure IR camera | **Laptops only** - For face recognition (e.g., Howdy). |
| `fingerprint.md` | Configure fingerprint reader | **Laptops only** - For biometric authentication. |

---

## Post-Installation Enhancements (After First Boot)

| Step | Purpose | When to Use |
|--------|---------|-------------|
| `gnome.md` | Install GNOME Desktop | **Optional** - If you prefer the GNOME desktop environment. |
| `kde-plasma.md` | Install KDE Plasma Desktop | **Optional** - If you prefer the KDE Plasma desktop environment. |
| `xfce.md` | Install XFCE Desktop | **Optional** - If you prefer the XFCE desktop environment. |
| `xorg-config.md` | Xorg Display Server config | **Optional** - If using Xorg-based DEs/WMs. |
| `wayland-config.md` | Wayland Display Server config | **Optional** - If using Wayland-based DEs/WMs. |
| `essential-applications.md` | Install essential applications | **Recommended** - For basic daily computing tasks. |
| `timeshift.md` | Timeshift for system snapshots | **Recommended** - For system backup and restore. |
| `w3m.md` | w3m Terminal Web Browser | **Optional** - For text-based web browsing in console/minimal environments. |
| `nvidia-drivers.md` | NVIDIA GPU Driver Installation | **Optional** - For systems with NVIDIA graphics. |
| `amd-drivers.md` | AMD GPU Driver Installation | **Optional** - For systems with AMD graphics. |

---

## Final Steps (Required)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `exit-chroot.md` | Exit chroot and reboot | **Always** - Final step before first boot into your new system. |
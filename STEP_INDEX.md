# Step Index - Individual Command Reference

**Purpose:** This index provides a detailed, granular list of individual installation steps (usually single commands or small sets of commands) that make up the modules. It's designed for quick reference, debugging, or for experienced users building highly customized installation procedures from scratch.

**Recommendation:** For a structured installation, it's highly recommended to follow the **[Installation Flows](wiki/INSTALLATION-FLOWS.md)** or **[Phases](phases/00-PREPARATION.md)** instead of picking individual steps directly from here.

---

## Installation Phases (Overview)

For an organized installation, you can follow the structured phases. Each phase links to a detailed guide.

1.  **[Phase 00: Preparation](phases/00-PREPARATION.md)** - USB creation, Windows installation, disk formatting
2.  **[Phase 01: Disk Setup](phases/01-DISK_SETUP.md)** - Partitioning, encryption, filesystem
3.  **[Phase 02: System Install](phases/02-SYSTEM_INSTALL.md)** - Base system installation, chroot
4.  **[Phase 03: Basic Config](phases/03-BASIC_CONFIG.md)** - Locale, user creation, sudo
5.  **[Phase 04: Bootloader](phases/04-BOOTLOADER.md)** - GRUB installation, LUKS auto-unlock
6.  **[Phase 05: Network](phases/05-NETWORK.md)** - NetworkManager, WiFi, Bluetooth
7.  **[Phase 06: Audio](phases/06-AUDIO.md)** - PipeWire audio
8.  **[Phase 07: Security](phases/07-SECURITY.md)** - SSH, UFW, fail2ban
9.  **[Phase 08: Laptop Hardware](phases/08-LAPTOP_HARDWARE.md)** - Touchpad, webcam, IR camera, fingerprint
10. **[Phase 09: Finalize](phases/09-FINALIZE.md)** - Exit chroot, reboot

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
| `00-core-installation.md` | Install base Arch Linux system | **Always** - This is the core installation. |
| `01-chroot.md` | Enter chroot environment | **Always** - Required for all configuration steps inside the new system. |

---

## Basic System Configuration (Recommended)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `02-locale.md` | Set system locale and timezone | **Recommended** - Set your language and timezone. |
| `04-user-creation.md` | Create user account | **Recommended** - Create non-root user with sudo access. |

---

## Bootloader (Required)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `05-grub.md` | Install GRUB bootloader | **Required** - System won't boot without this. |

---

## Disk and Filesystem Management (Choose One or More)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `06-disk-partitioning.md` | Create disk partitions | **If needed** - Only if disk is not already partitioned. |
| `07-luks-encryption.md` | Encrypt partitions | **Optional** - If you want full disk encryption. |
| `08-btrfs-filesystem.md` | Create Btrfs with subvolumes | **Optional** - If you want to use Btrfs filesystem. |
| `09-mount-partitions.md` | Mount partitions (non-Btrfs) | **If needed** - If using Ext4/XFS instead of Btrfs. |

---

## Advanced System Configuration (Optional)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `10-luks-keyfile-auto-unlock.md` | Auto-unlock LUKS on boot | **Optional** - If you want automatic decryption at boot. |
| `11-networkmanager.md` | Enable NetworkManager | **Recommended** - For managing network connectivity. |
| `12-wifi.md` | WiFi support | **If needed** - If your system uses WiFi. |
| `13-bluetooth.md` | Bluetooth support | **If needed** - If your system has Bluetooth devices. |
| `14-audio.md` | Audio server (PipeWire) | **If needed** - If you require sound. |

---

## Security and Network Services (Optional)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `21-ssh-server.md` | Configure SSH server | **If needed** - For remote SSH access. |
| `22-ufw-firewall.md` | Configure UFW firewall | **Recommended** - For system protection. |
| `23-fail2ban.md` | Configure fail2ban for SSH | **Recommended** - To protect SSH from brute-force attacks. |

---

## Laptop Hardware Integration (Laptops Only)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `17-laptop-touchpad.md` | Configure touchpad/trackpad | **Laptops only** - For touchpad functionality. |
| `18-laptop-webcam.md` | Configure built-in webcam | **Laptops only** - For webcam functionality. |
| `19-laptop-ir-camera.md` | Configure IR camera | **Laptops only** - For face recognition (e.g., Howdy). |
| `20-laptop-fingerprint.md` | Configure fingerprint reader | **Laptops only** - For biometric authentication. |

---

## Post-Installation Enhancements (After First Boot)

| Step | Purpose | When to Use |
|--------|---------|-------------|
| `24-gnome.md` | Install GNOME Desktop | **Optional** - If you prefer the GNOME desktop environment. |
| `25-kde-plasma.md` | Install KDE Plasma Desktop | **Optional** - If you prefer the KDE Plasma desktop environment. |
| `26-xfce.md` | Install XFCE Desktop | **Optional** - If you prefer the XFCE desktop environment. |
| `27-xorg-config.md` | Xorg Display Server config | **Optional** - If using Xorg-based DEs/WMs. |
| `28-wayland-config.md` | Wayland Display Server config | **Optional** - If using Wayland-based DEs/WMs. |
| `29-essential-applications.md` | Install essential applications | **Recommended** - For basic daily computing tasks. |
| `30-timeshift.md` | Timeshift for system snapshots | **Recommended** - For system backup and restore. |
| `31-nvidia-drivers.md` | NVIDIA GPU Driver Installation | **Optional** - For systems with NVIDIA graphics. |
| `32-amd-drivers.md` | AMD GPU Driver Installation | **Optional** - For systems with AMD graphics. |

---

## Final Steps (Required)

| Step | Purpose | When to Use |
|------|---------|-------------|
| `15-exit-chroot.md` | Exit chroot and reboot | **Always** - Final step before first boot into your new system. |
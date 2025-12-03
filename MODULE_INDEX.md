# Module Index - Quick Reference

**Purpose:** This index provides a comprehensive list of all available installation and configuration modules, along with their purpose and when to use them. It serves as a quick reference to help you build a custom installation procedure or understand the components of pre-defined installation flows within the broader [Arch Linux Installation Guide](https://wiki.archlinux.org/title/Installation_guide).

**Before building your custom procedure:**
*   Review the **[Installation Flows](wiki/INSTALLATION-FLOWS.md)** to see if a pre-built scenario matches your needs.
*   Understand the **[Phases](phases/00-PREPARATION.md)** for a logical grouping of steps.

---

## Preparation Modules (Pre-Installation)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `PRE-01-create-arch-usb.md` | Create bootable Arch Linux USB drive | **Always** - Required before installation |
| `PRE-02-install-windows.md` | Install Windows for dual boot | **Dual boot only** - Install Windows before Arch Linux |
| `PRE-03-format-disk.md` | Completely wipe and format disk | **Fresh install only** - If you want to wipe entire disk |

**Note:**
- `PRE-01` is required for all installations.
- `PRE-02` is only for dual boot scenarios.
- `PRE-03` is optional (only if you want to completely wipe disk).

---

## Core Installation (Required)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `00-core-installation.md` | Install base Arch Linux system | **Always** - This is the core installation. |
| `01-chroot.md` | Enter chroot environment | **Always** - Required for all configuration steps inside the new system. |

---

## Basic System Configuration (Recommended)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `02-locale.md` | Set system locale and timezone | **Recommended** - Set your language and timezone. |
| `04-user-creation.md` | Create user account | **Recommended** - Create non-root user with sudo access. |

---

## Bootloader (Required)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `05-grub.md` | Install GRUB bootloader | **Required** - System won't boot without this. |

---

## Disk and Filesystem Management (Choose One or More)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `06-disk-partitioning.md` | Create disk partitions | **If needed** - Only if disk is not already partitioned. |
| `07-luks-encryption.md` | Encrypt partitions | **Optional** - If you want full disk encryption. |
| `08-btrfs-filesystem.md` | Create Btrfs with subvolumes | **Optional** - If you want to use Btrfs filesystem. |
| `09-mount-partitions.md` | Mount partitions (non-Btrfs) | **If needed** - If using Ext4/XFS instead of Btrfs. |

---

## Advanced System Configuration (Optional)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `10-luks-keyfile-auto-unlock.md` | Auto-unlock LUKS on boot | **Optional** - If you want automatic decryption at boot. |
| `11-networkmanager.md` | Enable NetworkManager | **Recommended** - For managing network connectivity. |
| `12-wifi.md` | WiFi support | **If needed** - If your system uses WiFi. |
| `13-bluetooth.md` | Bluetooth support | **If needed** - If your system has Bluetooth devices. |
| `14-audio.md` | Audio server (PipeWire) | **If needed** - If you require sound. |

---

## Security and Network Services (Optional)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `21-ssh-server.md` | Configure SSH server | **If needed** - For remote SSH access. |
| `22-ufw-firewall.md` | Configure UFW firewall | **Recommended** - For system protection. |
| `23-fail2ban.md` | Configure fail2ban for SSH | **Recommended** - To protect SSH from brute-force attacks. |

---

## Laptop Hardware Integration (Laptops Only)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `17-laptop-touchpad.md` | Configure touchpad/trackpad | **Laptops only** - For touchpad functionality. |
| `18-laptop-webcam.md` | Configure built-in webcam | **Laptops only** - For webcam functionality. |
| `19-laptop-ir-camera.md` | Configure IR camera | **Laptops only** - For face recognition (e.g., Howdy). |
| `20-laptop-fingerprint.md` | Configure fingerprint reader | **Laptops only** - For biometric authentication. |

---

## Post-Installation Enhancements (After First Boot)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `24-gnome.md` | Install GNOME Desktop | **Optional** - If you prefer the GNOME desktop environment. |
| `25-kde-plasma.md` | Install KDE Plasma Desktop | **Optional** - If you prefer the KDE Plasma desktop environment. |
| `26-xfce.md` | Install XFCE Desktop | **Optional** - If you prefer the XFCE desktop environment. |
| `27-xorg-config.md` | Xorg Display Server config | **Optional** - If using Xorg-based DEs/WMs. |
| `28-wayland-config.md` | Wayland Display Server config | **Optional** - If using Wayland-based DEs/WMs. |
| `29-essential-applications.md` | Install essential applications | **Recommended** - For basic daily computing tasks. |
| `30-timeshift.md` | Timeshift for system snapshots | **Recommended** - For system backup and restore. |

---

## Final Steps (Required)

| Module | Purpose | When to Use |
|--------|---------|-------------|
| `15-exit-chroot.md` | Exit chroot and reboot | **Always** - Final step before first boot into your new system. |

---

## Module Dependencies (General Guidelines)

*   **Most modules** require you to be in the `chroot` environment (after `01-chroot.md`).
*   Modules executed "After first boot" require user login and network connectivity.
*   **GRUB** (`05-grub.md`) requires partitions to be mounted (`09-mount-partitions.md` or `08-btrfs-filesystem.md`).
*   **LUKS modules** (`07-luks-encryption.md`, `10-luks-keyfile-auto-unlock.md`) depend on `06-disk-partitioning.md`.
*   **Desktop Environments/Display Servers** typically require user creation (`04-user-creation.md`) and network (`11-networkmanager.md`).
*   **AUR helper** (used by some laptop modules) requires `git` and user with `sudo`.

---

## How to Use

1.  **Start with the [Installation Flows](wiki/INSTALLATION-FLOWS.md)** to find a suitable pre-built scenario, or continue here to build a custom one.
2.  **Review this Module Index** to identify the specific components you need.
3.  **Consult each module's prerequisites** to ensure correct order and dependencies.
4.  **Execute modules sequentially**, verifying success after each step.
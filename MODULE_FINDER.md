# Module Finder

**Purpose:** Find modules based on your needs, hardware, and installation scenario.

---

## Find by Use Case

### I want to...

#### Install Arch Linux
- **Start here:** `core-installation.md` (required)
- **Then:** `chroot.md` (required)
- **Finally:** `exit-chroot.md` (required)

#### Encrypt my disk
- **Start:** `disk-partitioning.md`
- **Then:** `luks-encryption.md`
- **Optional:** `luks-keyfile-auto-unlock.md` (auto-unlock on boot)

#### Dual boot with Windows
- **First:** `PRE-install-windows.md`
- **Then:** `disk-partitioning.md` (create Arch partition)
- **Continue:** Normal installation flow

#### Set up WiFi
- **Prerequisite:** `networkmanager.md`
- **Then:** `wifi.md`

#### Install a desktop environment
- **Choose one:**
  - `gnome.md` - Modern, user-friendly
  - `kde-plasma.md` - Highly customizable
  - `xfce.md` - Lightweight, traditional
- **Prerequisite:** `xorg-config.md` or `wayland-config.md`

#### Install a window manager
- **Choose one:**
  - `i3wm.md` - Tiling WM (Xorg)
  - `hyprland.md` - Wayland compositor
- **Prerequisite:** `xorg-config.md` (for i3) or `wayland-config.md` (for Hyprland)

#### Configure laptop hardware
- `touchpad.md` - Touchpad configuration
- `webcam.md` - Webcam setup
- `ir-camera.md` - Face recognition (Howdy)
- `fingerprint.md` - Fingerprint reader

#### Set up security
- `ssh-server.md` - Remote access
- `ufw-firewall.md` - Firewall protection
- `fail2ban.md` - SSH brute-force protection

#### Install GPU drivers
- `nvidia-drivers.md` - NVIDIA graphics
- `amd-drivers.md` - AMD graphics

#### Back up my system
- `timeshift.md` - System snapshots

---

## Find by Hardware Type

### Desktop Computer
**Recommended modules:**
- `core-installation.md` (required)
- `chroot.md` (required)
- `locale.md`
- `user-creation.md`
- `grub.md` (required)
- `networkmanager.md`
- `audio.md`
- `xorg-config.md` or `wayland-config.md`
- Desktop environment (gnome/kde-plasma/xfce)
- `essential-applications.md`
- `timeshift.md`
- GPU drivers (if needed)

**Skip:**
- `touchpad.md`, `webcam.md`, `ir-camera.md`, `fingerprint.md` (laptop-only)

### Laptop
**Recommended modules:**
- All desktop modules above, plus:
- `wifi.md` (usually needed)
- `touchpad.md` (laptops)
- `webcam.md` (if built-in webcam)
- `ir-camera.md` (if IR camera present)
- `fingerprint.md` (if fingerprint reader present)

---

## Find by Installation Scenario

### Minimal Installation
**Modules:**
1. `core-installation.md`
2. `chroot.md`
3. `locale.md`
4. `user-creation.md`
5. `grub.md`
6. `networkmanager.md`
7. `exit-chroot.md`

### Encrypted Single Boot
**Modules:**
1. `disk-partitioning.md`
2. `luks-encryption.md`
3. `btrfs-filesystem.md`
4. `core-installation.md`
5. `chroot.md`
6. `locale.md`
7. `user-creation.md`
8. `grub.md`
9. `luks-keyfile-auto-unlock.md` (optional)
10. `networkmanager.md`
11. `exit-chroot.md`

### Dual Boot
**Modules:**
1. `PRE-install-windows.md`
2. `disk-partitioning.md`
3. `btrfs-filesystem.md` or `mount-partitions.md`
4. `core-installation.md`
5. `chroot.md`
6. `locale.md`
7. `user-creation.md`
8. `grub.md` (with os-prober)
9. `networkmanager.md`
10. `exit-chroot.md`

### Full Featured Installation
**All modules from encrypted single boot, plus:**
- `wifi.md`
- `bluetooth.md`
- `audio.md`
- `ssh-server.md`
- `ufw-firewall.md`
- `fail2ban.md`
- Desktop environment or window manager
- `essential-applications.md`
- `timeshift.md`
- GPU drivers

---

## Find by Category

### Required Modules
- `core-installation.md` - Base system
- `chroot.md` - Enter chroot
- `exit-chroot.md` - Final step

### Disk & Filesystem
- `disk-partitioning.md` - Create partitions
- `luks-encryption.md` - Encrypt disk
- `btrfs-filesystem.md` - Btrfs filesystem
- `mount-partitions.md` - Mount ext4/xfs

### System Configuration
- `locale.md` - Language and timezone
- `user-creation.md` - Create user
- `grub.md` - Bootloader

### Network
- `networkmanager.md` - Network management
- `wifi.md` - WiFi support
- `bluetooth.md` - Bluetooth support

### Security
- `ssh-server.md` - SSH access
- `ufw-firewall.md` - Firewall
- `fail2ban.md` - SSH protection

### Hardware (Laptops)
- `touchpad.md` - Touchpad
- `webcam.md` - Webcam
- `ir-camera.md` - IR camera
- `fingerprint.md` - Fingerprint

### Desktop Environments
- `gnome.md` - GNOME
- `kde-plasma.md` - KDE Plasma
- `xfce.md` - XFCE

### Window Managers
- `i3wm.md` - i3 tiling WM
- `hyprland.md` - Hyprland Wayland

### Applications & Tools
- `essential-applications.md` - Basic apps
- `timeshift.md` - System backup
- `w3m.md` - Terminal browser

### GPU Drivers
- `nvidia-drivers.md` - NVIDIA
- `amd-drivers.md` - AMD

---

## Find by Difficulty

### Beginner-Friendly
- `core-installation.md` - Well-documented
- `locale.md` - Simple configuration
- `user-creation.md` - Straightforward
- `networkmanager.md` - Easy setup
- `gnome.md` - User-friendly desktop

### Intermediate
- `disk-partitioning.md` - Requires understanding of partitions
- `luks-encryption.md` - Encryption concepts
- `grub.md` - Bootloader configuration
- `kde-plasma.md` - More configuration options

### Advanced
- `btrfs-filesystem.md` - Subvolumes and snapshots
- `luks-keyfile-auto-unlock.md` - Advanced encryption
- `i3wm.md` - Tiling window manager
- `hyprland.md` - Wayland compositor

---

## Find by Time Required

### Quick (< 5 minutes)
- `chroot.md` - 1 minute
- `networkmanager.md` - 1 minute
- `locale.md` - 2-3 minutes
- `user-creation.md` - 2-3 minutes

### Medium (5-15 minutes)
- `core-installation.md` - 20-40 minutes (download time)
- `grub.md` - 10-15 minutes
- `disk-partitioning.md` - 10-15 minutes
- `wifi.md` - 5-10 minutes

### Longer (15+ minutes)
- `luks-encryption.md` - 15-30 minutes
- `btrfs-filesystem.md` - 10-20 minutes
- Desktop environments - 15-30 minutes
- GPU drivers - 20-40 minutes

---

## Quick Decision Tree

```
Need to install Arch Linux?
├─ Disk already partitioned?
│  ├─ Yes → core-installation.md
│  └─ No → disk-partitioning.md → core-installation.md
│
Want encryption?
├─ Yes → luks-encryption.md
└─ No → Skip
│
Want Btrfs?
├─ Yes → btrfs-filesystem.md
└─ No → mount-partitions.md
│
After core-installation.md:
├─ chroot.md (required)
├─ locale.md (recommended)
├─ user-creation.md (recommended)
├─ grub.md (required)
└─ exit-chroot.md (required)
│
After first boot:
├─ Need WiFi? → wifi.md
├─ Need desktop? → Choose: gnome.md, kde-plasma.md, xfce.md
├─ Want WM? → Choose: i3wm.md, hyprland.md
└─ Need security? → ssh-server.md, ufw-firewall.md, fail2ban.md
```

---

## See Also

- [Module Index](MODULE_INDEX.md) - Complete alphabetical list
- [Module Dependencies](MODULE_DEPENDENCIES.md) - Dependency relationships
- [Quick Reference](QUICK_REFERENCE.md) - Cheat sheet
- [Installation Scenarios](INSTALLATION-SCENARIOS.md) - Pre-built scenarios
- [Generate Procedure](GENERATE_PROCEDURE.md) - Build custom procedure

---

**Back to:** [Repository Root](README.md)

# Module Dependency Graph

**Purpose:** This document shows the dependencies between modules, helping you understand which modules must be completed before others can be executed.

---

## Dependency Rules

1. **Prerequisites must be completed first** - Each module lists its prerequisites
2. **Core modules are always required** - `core-installation.md` and `chroot.md` are needed for most modules
3. **Some modules can run in parallel** - Modules that don't depend on each other can be done in any order
4. **Post-boot modules** - Some modules must be run after first boot (not in chroot)

---

## Core Dependency Chain

```
PRE-create-arch-usb.md
    ↓
PRE-install-windows.md (optional, dual boot only)
    ↓
PRE-format-disk.md (optional, fresh install only)
    ↓
disk-partitioning.md (if needed)
    ↓
luks-encryption.md (optional, if using encryption)
    ↓
btrfs-filesystem.md OR mount-partitions.md
    ↓
core-installation.md (REQUIRED)
    ↓
chroot.md (REQUIRED)
    ↓
[Configuration modules in chroot]
    ↓
exit-chroot.md (REQUIRED)
    ↓
[Post-boot modules]
```

---

## Module Dependencies

### Pre-Installation Modules
**No dependencies** - These are the starting point

- `PRE-create-arch-usb.md` - No dependencies
- `PRE-install-windows.md` - Depends on: `PRE-create-arch-usb.md`
- `PRE-format-disk.md` - No dependencies (optional)

---

### Disk Setup Modules

**Dependency Chain:**
```
disk-partitioning.md
    ↓
luks-encryption.md (optional)
    ↓
btrfs-filesystem.md OR mount-partitions.md
```

- `disk-partitioning.md` - Depends on: Booted from Live USB
- `luks-encryption.md` - Depends on: `disk-partitioning.md`
- `btrfs-filesystem.md` - Depends on: `disk-partitioning.md` (and optionally `luks-encryption.md`)
- `mount-partitions.md` - Depends on: `disk-partitioning.md` (and optionally `luks-encryption.md`)

---

### Core Installation Modules

**Dependency Chain:**
```
[Partitions mounted at /mnt]
    ↓
core-installation.md (REQUIRED)
    ↓
chroot.md (REQUIRED)
```

- `core-installation.md` - Depends on: Partitions mounted at `/mnt`
- `chroot.md` - Depends on: `core-installation.md`

---

### Configuration Modules (In Chroot)

**All require:** `chroot.md`

**Basic Configuration:**
- `locale.md` - Depends on: `chroot.md`
- `user-creation.md` - Depends on: `chroot.md`
- `grub.md` - Depends on: `chroot.md`, partitions mounted

**Advanced Configuration:**
- `luks-keyfile-auto-unlock.md` - Depends on: `chroot.md`, `luks-encryption.md`, `grub.md`
- `networkmanager.md` - Depends on: `chroot.md`
- `wifi.md` - Depends on: `chroot.md`, `networkmanager.md`
- `bluetooth.md` - Depends on: `chroot.md`, `networkmanager.md`
- `audio.md` - Depends on: `chroot.md`

**Final Step:**
- `exit-chroot.md` - Depends on: `chroot.md`, all chroot configuration complete

---

### Post-Boot Modules

**All require:** System booted, user logged in (not in chroot)

**Security:**
- `ssh-server.md` - Depends on: After first boot, `networkmanager.md`
- `ufw-firewall.md` - Depends on: After first boot, `networkmanager.md`
- `fail2ban.md` - Depends on: After first boot, `ssh-server.md`

**Hardware:**
- `touchpad.md` - Depends on: After first boot, laptop hardware
- `webcam.md` - Depends on: After first boot, laptop hardware
- `ir-camera.md` - Depends on: After first boot, laptop hardware, `webcam.md`
- `fingerprint.md` - Depends on: After first boot, laptop hardware

**Display Servers:**
- `xorg-config.md` - Depends on: After first boot, `user-creation.md`
- `wayland-config.md` - Depends on: After first boot, `user-creation.md`

**Desktop Environments:**
- `gnome.md` - Depends on: After first boot, `wayland-config.md` or `xorg-config.md`
- `kde-plasma.md` - Depends on: After first boot, `wayland-config.md` or `xorg-config.md`
- `xfce.md` - Depends on: After first boot, `xorg-config.md`
- `i3wm.md` - Depends on: After first boot, `xorg-config.md`
- `hyprland.md` - Depends on: After first boot, `wayland-config.md`

**Applications & Tools:**
- `essential-applications.md` - Depends on: After first boot, `networkmanager.md`
- `timeshift.md` - Depends on: After first boot, `networkmanager.md`
- `w3m.md` - Depends on: After first boot, `networkmanager.md`

**GPU Drivers:**
- `nvidia-drivers.md` - Depends on: After first boot, `networkmanager.md`
- `amd-drivers.md` - Depends on: After first boot, `networkmanager.md`

---

## Dependency Visualization

### Minimal Installation Flow
```
core-installation.md
    ↓
chroot.md
    ↓
locale.md
    ↓
user-creation.md
    ↓
grub.md
    ↓
networkmanager.md
    ↓
exit-chroot.md
```

### Full Installation Flow (Encrypted)
```
disk-partitioning.md
    ↓
luks-encryption.md
    ↓
btrfs-filesystem.md
    ↓
core-installation.md
    ↓
chroot.md
    ↓
locale.md
    ↓
user-creation.md
    ↓
grub.md
    ↓
luks-keyfile-auto-unlock.md
    ↓
networkmanager.md
    ↓
wifi.md
    ↓
audio.md
    ↓
exit-chroot.md
    ↓
[Post-boot modules]
```

---

## Parallel Execution

These modules can be run in parallel (same dependencies):

**In Chroot:**
- `locale.md` and `user-creation.md` (both need `chroot.md`)
- `networkmanager.md`, `wifi.md`, `bluetooth.md` (all need `chroot.md`, `networkmanager.md`)

**After Boot:**
- `ssh-server.md` and `ufw-firewall.md` (both need after boot, network)
- `touchpad.md`, `webcam.md`, `fingerprint.md` (all need after boot, hardware)
- `gnome.md`, `kde-plasma.md`, `xfce.md` (choose one, all need display server)

---

## Common Dependency Patterns

### Pattern 1: Encryption Setup
```
disk-partitioning.md → luks-encryption.md → btrfs-filesystem.md → core-installation.md
```

### Pattern 2: Network Setup
```
networkmanager.md → wifi.md (optional) → bluetooth.md (optional)
```

### Pattern 3: Desktop Environment
```
xorg-config.md OR wayland-config.md → [Choose DE/WM] → essential-applications.md
```

### Pattern 4: Security Stack
```
networkmanager.md → ssh-server.md → ufw-firewall.md → fail2ban.md
```

---

## Quick Reference

**Always Required:**
1. `core-installation.md`
2. `chroot.md`
3. `exit-chroot.md`

**Highly Recommended:**
- `locale.md`
- `user-creation.md`
- `grub.md`
- `networkmanager.md`

**Optional but Common:**
- `disk-partitioning.md` (if disk not partitioned)
- `luks-encryption.md` (if using encryption)
- `wifi.md` (if using WiFi)
- `audio.md` (if need sound)
- Desktop environment or window manager

---

**See Also:**
- [Module Index](module-index.md) - Complete list of all modules
- [Generate Procedure](generate-procedure.md) - How to build custom installation
- [Installation Scenarios](installation-scenarios.md) - Pre-built scenarios

---

**Back to:** [Repository Root](README.md)

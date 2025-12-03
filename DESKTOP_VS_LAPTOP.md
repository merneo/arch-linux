# Desktop vs Laptop - Module Selection Guide

**Purpose:** Help you choose the right modules based on your hardware type

---

## Quick Decision Tree

**Do you have a laptop or desktop?**

- **Laptop** → Has built-in touchpad, webcam, possibly fingerprint reader and IR camera
- **Desktop** → Uses external mouse, no built-in webcam (unless external), no fingerprint reader (unless external)

---

## Desktop Computer Modules

**Use these modules for desktop computers:**

### Required Modules (All Systems)
- `core-installation.md` - Install base system
- `chroot.md` - Enter chroot
- `grub.md` - Install GRUB
- `exit-chroot.md` - Exit and reboot

### Optional Modules (Desktop)
- `locale.md` - Set locale
- `user-creation.md` - Create user
- `disk-partitioning.md` - Partition disk (if needed)
- `luks-encryption.md` - Encrypt partitions
- `btrfs-filesystem.md` - Create Btrfs
- `luks-keyfile-auto-unlock.md` - Auto-unlock LUKS
- `networkmanager.md` - Enable NetworkManager
- `wifi.md` - WiFi (if using WiFi adapter)
- `bluetooth.md` - Bluetooth (if using Bluetooth adapter)
- `audio.md` - Audio server
- `xorg-config.md` - Xorg Display Server config
- `wayland-config.md` - Wayland Display Server config
- `essential-applications.md` - Install essential applications
- `timeshift.md` - Timeshift for system snapshots
- `nvidia-drivers.md` - NVIDIA GPU Driver Installation (if using NVIDIA)
- `amd-drivers.md` - AMD GPU Driver Installation (if using AMD)

### Skip These Modules (Desktop)
- ❌ `touchpad.md` - Desktop uses external mouse
- ❌ `webcam.md` - Desktop doesn't have built-in webcam (use external USB webcam if needed)
- ❌ `ir-camera.md` - Desktop doesn't have IR camera
- ❌ `fingerprint.md` - Desktop doesn't have fingerprint reader (unless external USB)

---

## Laptop Modules

**Use these modules for laptops:**

### Required Modules (All Systems)
- `core-installation.md` - Install base system
- `chroot.md` - Enter chroot
- `grub.md` - Install GRUB
- `exit-chroot.md` - Exit and reboot

### Optional Modules (Laptop)
- `locale.md` - Set locale
- `user-creation.md` - Create user
- `disk-partitioning.md` - Partition disk (if needed)
- `luks-encryption.md` - Encrypt partitions
- `btrfs-filesystem.md` - Create Btrfs
- `luks-keyfile-auto-unlock.md` - Auto-unlock LUKS
- `networkmanager.md` - Enable NetworkManager
- `wifi.md` - WiFi (laptops usually have WiFi)
- `bluetooth.md` - Bluetooth (laptops usually have Bluetooth)
- `audio.md` - Audio server
- `xorg-config.md` - Xorg Display Server config
- `wayland-config.md` - Wayland Display Server config
- `essential-applications.md` - Install essential applications
- `timeshift.md` - Timeshift for system snapshots
- `nvidia-drivers.md` - NVIDIA GPU Driver Installation (if using NVIDIA)
- `amd-drivers.md` - AMD GPU Driver Installation (if using AMD)

### Laptop-Specific Modules (After First Boot)
- ✅ `touchpad.md` - **Recommended** - Configure touchpad
- ✅ `webcam.md` - **Recommended** - Configure webcam (if present)
- ✅ `ir-camera.md` - **Optional** - Configure IR camera (if present, business laptops)
- ✅ `fingerprint.md` - **Optional** - Configure fingerprint reader (if present)

---

## Hardware Detection

To determine if specific hardware is detected and how it's named in your system, you can use various command-line tools:

### Check for Touchpad
```bash
libinput list-devices | grep -i touchpad # See [ArchWiki: libinput](https://wiki.archlinux.org/title/Libinput).
```

### Check for Webcam
```bash
lsusb | grep -i camera # See [ArchWiki: lsusb](https://wiki.archlinux.org/title/USB#lsusb).
ls -la /dev/video* # See [ArchWiki: Webcam setup](https://wiki.archlinux.org/title/Webcam_setup).
```

### Check for IR Camera
```bash
lsusb | grep -i "ir\|camera" # See [ArchWiki: lsusb](https://wiki.archlinux.org/title/USB#lsusb).
# IR cameras usually have separate device ID from regular webcam
```

### Check for Fingerprint Reader
```bash
lsusb | grep -i "fingerprint\|validity" # See [ArchWiki: lsusb](https://wiki.archlinux.org/title/USB#lsusb).
```

---

## Example Installation Flows

### Desktop Computer (Minimal)
1. `core-installation.md`
2. `chroot.md`
3. `grub.md`
4. `networkmanager.md` (if using Ethernet)
5. `exit-chroot.md`
6. **After first boot:**
   - `xorg-config.md` or `wayland-config.md` (display server)
   - `essential-applications.md` (web browser, etc.)

### Desktop Computer (Full)
1. `disk-partitioning.md` (if needed)
2. `luks-encryption.md`
3. `btrfs-filesystem.md`
4. `core-installation.md`
5. `chroot.md`
6. `locale.md`
7. `user-creation.md`
8. `grub.md`
9. `luks-keyfile-auto-unlock.md`
10. `networkmanager.md`
11. `audio.md`
12. `exit-chroot.md`
13. **After first boot:**
    - `xorg-config.md` or `wayland-config.md`
    - `gnome.md`, `kde-plasma.md`, or `xfce.md` (desktop environment)
    - `essential-applications.md`
    - `timeshift.md` (backup)
    - `nvidia-drivers.md` or `amd-drivers.md` (GPU drivers)

### Laptop (Minimal)
1. `core-installation.md`
2. `chroot.md`
3. `grub.md`
4. `networkmanager.md`
5. `wifi.md`
6. `exit-chroot.md`
7. **After first boot:**
   - `touchpad.md`
   - `webcam.md`
   - `xorg-config.md` or `wayland-config.md` (display server)
   - `essential-applications.md` (web browser, etc.)

### Laptop (Full with Biometrics)
1. `disk-partitioning.md` (if needed)
2. `luks-encryption.md`
3. `btrfs-filesystem.md`
4. `core-installation.md`
5. `chroot.md`
6. `locale.md`
7. `user-creation.md`
8. `grub.md`
9. `luks-keyfile-auto-unlock.md`
10. `networkmanager.md`
11. `wifi.md`
12. `bluetooth.md`
13. `audio.md`
14. `exit-chroot.md`
15. **After first boot:**
    - `touchpad.md`
    - `webcam.md`
    - `ir-camera.md` (if IR camera present)
    - `fingerprint.md` (if fingerprint reader present)
    - `xorg-config.md` or `wayland-config.md`
    - `gnome.md`, `kde-plasma.md`, or `xfce.md`
    - `essential-applications.md`
    - `timeshift.md`
    - `nvidia-drivers.md` or `amd-drivers.md`

---

## Summary

| Feature | Desktop | Laptop |
|---------|---------|--------|
| **Touchpad** | ❌ No (use external mouse) | ✅ Yes (use `touchpad.md`) |
| **Webcam** | ❌ No built-in (use external USB) | ✅ Yes (use `webcam.md`) |
| **IR Camera** | ❌ No | ✅ Maybe (business laptops, use `ir-camera.md`) |
| **Fingerprint** | ❌ No built-in (use external USB) | ✅ Maybe (use `fingerprint.md`) |
| **WiFi** | ❌ Maybe (use WiFi adapter) | ✅ Usually yes (use `wifi.md`) |
| **Bluetooth** | ❌ Maybe (use Bluetooth adapter) | ✅ Usually yes (use `bluetooth.md`) |

---

**Remember:** Laptop hardware modules (`17-20`) should be configured **after first boot**, not during chroot installation.

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
- `00-core-installation.md` - Install base system
- `01-chroot.md` - Enter chroot
- `05-grub.md` - Install GRUB
- `15-exit-chroot.md` - Exit and reboot

### Optional Modules (Desktop)
- `02-locale.md` - Set locale
- `04-user-creation.md` - Create user
- `06-disk-partitioning.md` - Partition disk (if needed)
- `07-luks-encryption.md` - Encrypt partitions
- `08-btrfs-filesystem.md` - Create Btrfs
- `10-luks-keyfile-auto-unlock.md` - Auto-unlock LUKS
- `11-networkmanager.md` - Enable NetworkManager
- `12-wifi.md` - WiFi (if using WiFi adapter)
- `13-bluetooth.md` - Bluetooth (if using Bluetooth adapter)
- `14-audio.md` - Audio server
- `27-xorg-config.md` - Xorg Display Server config
- `28-wayland-config.md` - Wayland Display Server config
- `29-essential-applications.md` - Install essential applications
- `30-timeshift.md` - Timeshift for system snapshots
- `31-nvidia-drivers.md` - NVIDIA GPU Driver Installation (if using NVIDIA)
- `32-amd-drivers.md` - AMD GPU Driver Installation (if using AMD)

### Skip These Modules (Desktop)
- ❌ `17-laptop-touchpad.md` - Desktop uses external mouse
- ❌ `18-laptop-webcam.md` - Desktop doesn't have built-in webcam (use external USB webcam if needed)
- ❌ `19-laptop-ir-camera.md` - Desktop doesn't have IR camera
- ❌ `20-laptop-fingerprint.md` - Desktop doesn't have fingerprint reader (unless external USB)

---

## Laptop Modules

**Use these modules for laptops:**

### Required Modules (All Systems)
- `00-core-installation.md` - Install base system
- `01-chroot.md` - Enter chroot
- `05-grub.md` - Install GRUB
- `15-exit-chroot.md` - Exit and reboot

### Optional Modules (Laptop)
- `02-locale.md` - Set locale
- `04-user-creation.md` - Create user
- `06-disk-partitioning.md` - Partition disk (if needed)
- `07-luks-encryption.md` - Encrypt partitions
- `08-btrfs-filesystem.md` - Create Btrfs
- `10-luks-keyfile-auto-unlock.md` - Auto-unlock LUKS
- `11-networkmanager.md` - Enable NetworkManager
- `12-wifi.md` - WiFi (laptops usually have WiFi)
- `13-bluetooth.md` - Bluetooth (laptops usually have Bluetooth)
- `14-audio.md` - Audio server
- `27-xorg-config.md` - Xorg Display Server config
- `28-wayland-config.md` - Wayland Display Server config
- `29-essential-applications.md` - Install essential applications
- `30-timeshift.md` - Timeshift for system snapshots
- `31-nvidia-drivers.md` - NVIDIA GPU Driver Installation (if using NVIDIA)
- `32-amd-drivers.md` - AMD GPU Driver Installation (if using AMD)

### Laptop-Specific Modules (After First Boot)
- ✅ `17-laptop-touchpad.md` - **Recommended** - Configure touchpad
- ✅ `18-laptop-webcam.md` - **Recommended** - Configure webcam (if present)
- ✅ `19-laptop-ir-camera.md` - **Optional** - Configure IR camera (if present, business laptops)
- ✅ `20-laptop-fingerprint.md` - **Optional** - Configure fingerprint reader (if present)

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
1. `00-core-installation.md`
2. `01-chroot.md`
3. `05-grub.md`
4. `11-networkmanager.md` (if using Ethernet)
5. `15-exit-chroot.md`
6. **After first boot:**
   - `27-xorg-config.md` or `28-wayland-config.md` (display server)
   - `29-essential-applications.md` (web browser, etc.)

### Desktop Computer (Full)
1. `06-disk-partitioning.md` (if needed)
2. `07-luks-encryption.md`
3. `08-btrfs-filesystem.md`
4. `00-core-installation.md`
5. `01-chroot.md`
6. `02-locale.md`
7. `04-user-creation.md`
8. `05-grub.md`
9. `10-luks-keyfile-auto-unlock.md`
10. `11-networkmanager.md`
11. `14-audio.md`
12. `15-exit-chroot.md`
13. **After first boot:**
    - `27-xorg-config.md` or `28-wayland-config.md`
    - `24-gnome.md`, `25-kde-plasma.md`, or `26-xfce.md` (desktop environment)
    - `29-essential-applications.md`
    - `30-timeshift.md` (backup)
    - `31-nvidia-drivers.md` or `32-amd-drivers.md` (GPU drivers)

### Laptop (Minimal)
1. `00-core-installation.md`
2. `01-chroot.md`
3. `05-grub.md`
4. `11-networkmanager.md`
5. `12-wifi.md`
6. `15-exit-chroot.md`
7. **After first boot:**
   - `17-laptop-touchpad.md`
   - `18-laptop-webcam.md`
   - `27-xorg-config.md` or `28-wayland-config.md` (display server)
   - `29-essential-applications.md` (web browser, etc.)

### Laptop (Full with Biometrics)
1. `06-disk-partitioning.md` (if needed)
2. `07-luks-encryption.md`
3. `08-btrfs-filesystem.md`
4. `00-core-installation.md`
5. `01-chroot.md`
6. `02-locale.md`
7. `04-user-creation.md`
8. `05-grub.md`
9. `10-luks-keyfile-auto-unlock.md`
10. `11-networkmanager.md`
11. `12-wifi.md`
12. `13-bluetooth.md`
13. `14-audio.md`
14. `15-exit-chroot.md`
15. **After first boot:**
    - `17-laptop-touchpad.md`
    - `18-laptop-webcam.md`
    - `19-laptop-ir-camera.md` (if IR camera present)
    - `20-laptop-fingerprint.md` (if fingerprint reader present)
    - `27-xorg-config.md` or `28-wayland-config.md`
    - `24-gnome.md`, `25-kde-plasma.md`, or `26-xfce.md`
    - `29-essential-applications.md`
    - `30-timeshift.md`
    - `31-nvidia-drivers.md` or `32-amd-drivers.md`

---

## Summary

| Feature | Desktop | Laptop |
|---------|---------|--------|
| **Touchpad** | ❌ No (use external mouse) | ✅ Yes (use `17-laptop-touchpad.md`) |
| **Webcam** | ❌ No built-in (use external USB) | ✅ Yes (use `18-laptop-webcam.md`) |
| **IR Camera** | ❌ No | ✅ Maybe (business laptops, use `19-laptop-ir-camera.md`) |
| **Fingerprint** | ❌ No built-in (use external USB) | ✅ Maybe (use `20-laptop-fingerprint.md`) |
| **WiFi** | ❌ Maybe (use WiFi adapter) | ✅ Usually yes (use `12-wifi.md`) |
| **Bluetooth** | ❌ Maybe (use Bluetooth adapter) | ✅ Usually yes (use `13-bluetooth.md`) |

---

**Remember:** Laptop hardware modules (`17-20`) should be configured **after first boot**, not during chroot installation.

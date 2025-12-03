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
- `03-root-password.md` - Set root password
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
- `03-root-password.md` - Set root password
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

### Laptop-Specific Modules (After First Boot)
- ✅ `17-laptop-touchpad.md` - **Recommended** - Configure touchpad
- ✅ `18-laptop-webcam.md` - **Recommended** - Configure webcam (if present)
- ✅ `19-laptop-ir-camera.md` - **Optional** - Configure IR camera (if present, business laptops)
- ✅ `20-laptop-fingerprint.md` - **Optional** - Configure fingerprint reader (if present)

---

## Hardware Detection

**How to check what hardware you have:**

### Check for Touchpad
```bash
libinput list-devices | grep -i touchpad
```

### Check for Webcam
```bash
lsusb | grep -i camera
ls -la /dev/video*
```

### Check for IR Camera
```bash
lsusb | grep -i "ir\|camera"
# IR cameras usually have separate device ID from regular webcam
```

### Check for Fingerprint Reader
```bash
lsusb | grep -i "fingerprint\|validity"
```

---

## Example Installation Flows

### Desktop Computer (Minimal)
1. `00-core-installation.md`
2. `01-chroot.md`
3. `03-root-password.md`
4. `05-grub.md`
5. `11-networkmanager.md` (if using Ethernet)
6. `15-exit-chroot.md`

### Desktop Computer (Full)
1. `06-disk-partitioning.md` (if needed)
2. `07-luks-encryption.md`
3. `08-btrfs-filesystem.md`
4. `00-core-installation.md`
5. `01-chroot.md`
6. `02-locale.md`
7. `03-root-password.md`
8. `04-user-creation.md`
9. `05-grub.md`
10. `10-luks-keyfile-auto-unlock.md`
11. `11-networkmanager.md`
12. `14-audio.md`
13. `15-exit-chroot.md`

### Laptop (Minimal)
1. `00-core-installation.md`
2. `01-chroot.md`
3. `03-root-password.md`
4. `05-grub.md`
5. `11-networkmanager.md`
6. `12-wifi.md`
7. `15-exit-chroot.md`
8. **After first boot:**
   - `17-laptop-touchpad.md`
   - `18-laptop-webcam.md`

### Laptop (Full with Biometrics)
1. `06-disk-partitioning.md` (if needed)
2. `07-luks-encryption.md`
3. `08-btrfs-filesystem.md`
4. `00-core-installation.md`
5. `01-chroot.md`
6. `02-locale.md`
7. `03-root-password.md`
8. `04-user-creation.md`
9. `05-grub.md`
10. `10-luks-keyfile-auto-unlock.md`
11. `11-networkmanager.md`
12. `12-wifi.md`
13. `13-bluetooth.md`
14. `14-audio.md`
15. `15-exit-chroot.md`
16. **After first boot:**
    - `17-laptop-touchpad.md`
    - `18-laptop-webcam.md`
    - `19-laptop-ir-camera.md` (if IR camera present)
    - `20-laptop-fingerprint.md` (if fingerprint reader present)

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

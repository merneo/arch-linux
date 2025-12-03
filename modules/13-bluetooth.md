# Module: Bluetooth Setup

**Purpose:** Install and enable Bluetooth support

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)

**Time:** 2-3 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## Step 1: Install Bluetooth Packages

```bash
pacman -S bluez bluez-utils
```

**Package breakdown:**
- `bluez` - Bluetooth protocol stack
- `bluez-utils` - bluetoothctl and other tools

---

## Step 2: Enable Bluetooth Service

```bash
systemctl enable bluetooth
```

**Output:**
```
Created symlink /etc/systemd/system/dbus-org.bluez.service → /usr/lib/systemd/system/bluetooth.service
```

---

**SUCCESS:** Bluetooth will start automatically on boot

**After reboot:**
- Use `bluetoothctl` to pair devices
- Or use Bluetooth GUI (if desktop environment installed)

**Example usage:**
```bash
bluetoothctl
power on
scan on
pair <device-address>
connect <device-address>
```

---

**Next:** Continue with other configuration modules

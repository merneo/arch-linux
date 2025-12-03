# Module: Bluetooth Setup

**Purpose:** Install and enable Bluetooth support. Bluetooth is a wireless technology standard for exchanging data over short distances. For a comprehensive guide on setting up Bluetooth in Arch Linux, refer to the [ArchWiki on Bluetooth](https://wiki.archlinux.org/title/Bluetooth).

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
- `bluez`: The official Linux Bluetooth protocol stack, providing core Bluetooth functionality. See [ArchWiki: BlueZ](https://wiki.archlinux.org/title/Bluetooth#BlueZ).
- `bluez-utils`: Provides utilities like `bluetoothctl`, which is the primary command-line tool for managing Bluetooth devices.

---

## Step 2: Enable Bluetooth Service

The `systemctl enable bluetooth` command ensures that the Bluetooth service starts automatically at boot, managed by `systemd`. For more details on `systemd` and service management, refer to the [ArchWiki on systemd](https://wiki.archlinux.org/title/Systemd).

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
- Use `bluetoothctl` to pair devices. `bluetoothctl` is the primary command-line tool for managing Bluetooth devices and is part of `bluez-utils`. For its usage, see [ArchWiki: Bluetooth#Pairing](https://wiki.archlinux.org/title/Bluetooth#Pairing).
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

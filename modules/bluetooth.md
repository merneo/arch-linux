# Module: Bluetooth Setup

**Purpose:** Install and enable Bluetooth support. [Bluetooth](https://wiki.archlinux.org/title/Bluetooth) is a short-range wireless technology standard for exchanging data between fixed and mobile devices, enabling features like wireless audio, file transfer, and peripheral connectivity. The core software stack on Linux is provided by BlueZ.

**Prerequisites:**
- Inside chroot environment (module `chroot.md`)

**Time:** 2-3 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## How to know if you have a Bluetooth adapter?

Most modern laptops have an integrated Bluetooth adapter, and many desktop motherboards include it as well. You can confirm its presence and identify its chipset by:
-   **Physical inspection:** Look for a Bluetooth logo on your device or its specifications.
-   **Device specifications:** Check your laptop's or motherboard's model specifications online.
-   **System detection commands:** Use `lspci` (for PCI-E/internal cards) and `lsusb` (for USB adapters) to identify the adapter.

```bash
# For PCI-E based Bluetooth adapters (often integrated with WiFi):
lspci -k | grep -EA3 'Bluetooth|Wireless'

# For USB Bluetooth adapters:
lsusb | grep -i bluetooth
```

---

## Step 1: Install Bluetooth Packages

Identifying your Bluetooth chipset (using `lspci` or `lsusb` as shown above) can be important for troubleshooting, though most adapters work out-of-the-box with `bluez`. Some adapters (e.g., Intel) require specific firmware provided by the `linux-firmware` package.

```bash
pacman -S bluez bluez-utils
```

**Package breakdown:**
- `bluez`: The official Linux Bluetooth protocol stack, providing core Bluetooth functionality. See [ArchWiki: BlueZ](https://wiki.archlinux.org/title/Bluetooth#BlueZ).
- `bluez-utils`: Provides utilities like `bluetoothctl`, which is the primary command-line tool for managing Bluetooth devices.


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

## Troubleshooting

For more extensive troubleshooting on Bluetooth issues, refer to the [ArchWiki on Bluetooth#Troubleshooting](https://wiki.archlinux.org/title/Bluetooth#Troubleshooting).

### Problem: Bluetooth adapter not detected or not working
**Solution:**
1. **Verify hardware detection:**
   - For PCI-E cards: `lspci -k | grep -EA3 'Bluetooth|Wireless'`
   - For USB adapters: `lsusb | grep -i bluetooth`
   - Check if kernel module is loaded: `lsmod | grep btusb` (for USB adapters) or `lsmod | grep btintel` (for Intel integrated).
2. **Check dmesg for errors:** `dmesg | grep -i 'bluetooth\|firmware'`
3. **Ensure `bluez` service is running:** `systemctl status bluetooth`
4. **Ensure adapter is not soft/hard blocked:** `rfkill list all` (unblock if necessary).
5. **Reboot:** Sometimes a fresh boot helps load drivers/firmware correctly.

### Problem: Pairing issues
**Solution:**
1. **Ensure device is in pairing mode.**
2. **Check adapter visibility:** `bluetoothctl discoverable on`, `bluetoothctl agent on`, `bluetoothctl default-agent`.
3. **Remove existing pairings:** `bluetoothctl remove <device-address>`.
4. **Restart Bluetooth service:** `sudo systemctl restart bluetooth`.

---

**Next:** Continue with other configuration modules

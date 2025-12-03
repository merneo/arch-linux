# Step: Bluetooth Setup

**Purpose:** Install and enable Bluetooth support. For a comprehensive guide on setting up Bluetooth in Arch Linux, refer to the [ArchWiki on Bluetooth](https://wiki.archlinux.org/title/Bluetooth).

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment

## Commands

```bash
# Install Bluetooth packages. See [ArchWiki: BlueZ](https://wiki.archlinux.org/title/Bluetooth#BlueZ).
pacman -S bluez bluez-utils

# Enable Bluetooth service. See [ArchWiki: systemd](https://wiki.archlinux.org/title/Systemd).
systemctl enable bluetooth
```

**After reboot, use:** For `bluetoothctl` usage, see [ArchWiki: Bluetooth#Pairing](https://wiki.archlinux.org/title/Bluetooth#Pairing).
```bash
bluetoothctl
power on
scan on
pair <device-address>
connect <device-address>
```

**NEXT:** `14-audio.md` or continue with other modules

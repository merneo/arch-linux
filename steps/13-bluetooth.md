# Step: Bluetooth Setup

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment

## Commands

```bash
# Install Bluetooth packages
pacman -S bluez bluez-utils

# Enable Bluetooth service
systemctl enable bluetooth
```

**After reboot, use:**
```bash
bluetoothctl
power on
scan on
pair <device-address>
connect <device-address>
```

**NEXT:** `14-audio.md` or continue with other modules

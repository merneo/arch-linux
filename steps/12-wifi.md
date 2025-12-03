# Step: WiFi Configuration

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment, NetworkManager enabled

## Commands

```bash
# Install WiFi packages
pacman -S wireless_tools wpa_supplicant dialog
```

**After reboot, connect to WiFi:**
```bash
nmcli device wifi list
nmcli device wifi connect "SSID" password "password"
# Or use: nmtui
```

**NEXT:** `13-bluetooth.md` (if using Bluetooth) or continue with other modules

# Step: WiFi Configuration

**Purpose:** Install and configure WiFi support. For a comprehensive guide on wireless networking in Arch Linux, refer to the [ArchWiki on Wireless network configuration](https://wiki.archlinux.org/title/Wireless_network_configuration).

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment, NetworkManager enabled

## Commands

```bash
# Install WiFi packages. See [ArchWiki: Wireless tools](https://wiki.archlinux.org/title/Wireless_tools) and [ArchWiki: wpa_supplicant](https://wiki.archlinux.org/title/WPA_supplicant).
pacman -S wireless_tools wpa_supplicant dialog
```

**After reboot, connect to WiFi:** See [ArchWiki: nmcli](https://wiki.archlinux.org/title/NetworkManager#nmcli) and [ArchWiki: nmtui](https://wiki.archlinux.org/title/NetworkManager#nmtui).
```bash
nmcli device wifi list
nmcli device wifi connect "SSID" password "password"
# Or use: nmtui
```

**NEXT:** `bluetooth.md` (if using Bluetooth) or continue with other modules

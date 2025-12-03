# Step: NetworkManager Setup

**Purpose:** Enable NetworkManager for network connectivity. For comprehensive details, refer to the [ArchWiki on NetworkManager](https://wiki.archlinux.org/title/NetworkManager).

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment, NetworkManager installed

## Commands

```bash
systemctl enable NetworkManager # Ensures NetworkManager starts automatically on boot. See [ArchWiki: systemd](https://wiki.archlinux.org/title/Systemd).
```

**NEXT:** `wifi.md` (if using WiFi) or continue with other modules

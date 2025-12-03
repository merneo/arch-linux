# Module: NetworkManager Setup

**Purpose:** Enable NetworkManager for network connectivity. NetworkManager is a daemon that dynamically controls and configures network connections, aiming to keep network connectivity active whenever possible. For comprehensive details, refer to the [ArchWiki on NetworkManager](https://wiki.archlinux.org/title/NetworkManager).

**Prerequisites:**
- Inside chroot environment (module `chroot.md`)
- NetworkManager installed (included in `core-installation.md`)

**Time:** 1 minute

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## Enable NetworkManager Service

Services in Arch Linux are managed by `systemd`, the init system. The `systemctl enable` command creates a symlink from the systemd unit file to the appropriate runlevel directory, ensuring the service starts on boot. For more details on `systemd` and `systemctl`, refer to the [ArchWiki on systemd](https://wiki.archlinux.org/title/Systemd).

```bash
systemctl enable NetworkManager
```

**Output:**
```
Created symlink /etc/systemd/system/multi-user.target.wants/NetworkManager.service → /usr/lib/systemd/system/NetworkManager.service
```

---

**SUCCESS:** NetworkManager will start automatically on boot

**After reboot:**
- NetworkManager will automatically connect to wired networks
- Use `nmcli` (NetworkManager command line interface) or `nmtui` (NetworkManager Text User Interface) to configure WiFi. For details on these tools, refer to the [ArchWiki on NetworkManager#Network_configuration](https://wiki.archlinux.org/title/NetworkManager#Network_configuration).
- Or use NetworkManager GUI applet

---

## Troubleshooting

For more extensive troubleshooting on NetworkManager, refer to the [ArchWiki on NetworkManager#Troubleshooting](https://wiki.archlinux.org/title/NetworkManager#Troubleshooting).

### Problem: NetworkManager service fails to start
**Solution:**
1. Check service status: `systemctl status NetworkManager`
2. Verify NetworkManager is installed: `pacman -Q networkmanager`
3. Check for conflicting services: `systemctl is-active dhcpcd` (should be inactive)
4. Enable service: `systemctl enable --now NetworkManager`

### Problem: No network connection after reboot
**Solution:**
1. Check NetworkManager status: `systemctl status NetworkManager`
2. List network interfaces: `ip link show`
3. Check connection status: `nmcli connection show`
4. Try connecting manually: `nmcli device connect <interface>`

### Problem: Cannot configure WiFi
**Solution:**
1. Install WiFi tools: `pacman -S iw wpa_supplicant`
2. Use `nmtui` for interactive setup: `nmtui`
3. Or use `nmcli`: `nmcli device wifi list` then `nmcli device wifi connect <SSID> password <PASSWORD>`

---

**Next:** Continue with other configuration modules

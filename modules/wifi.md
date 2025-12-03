# Module: WiFi Configuration

**Purpose:** Install and configure WiFi support. For a comprehensive guide on wireless networking in Arch Linux, refer to the [ArchWiki on Wireless network configuration](https://wiki.archlinux.org/title/Wireless_network_configuration).

**Prerequisites:**
- Inside chroot environment (module `chroot.md`)
- NetworkManager enabled (module `networkmanager.md`)

**Time:** 2-3 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## Step 1: Install WiFi Packages

```bash
pacman -S wireless_tools wpa_supplicant dialog
```

**Package breakdown:**
- `wireless_tools`: A collection of utilities for configuring wireless network interfaces. See [ArchWiki: Wireless tools](https://wiki.archlinux.org/title/Wireless_tools).
- `wpa_supplicant`: An implementation of the WPA Supplicant, which is a key component for WPA/WPA2 authentication. See [ArchWiki: wpa_supplicant](https://wiki.archlinux.org/title/WPA_supplicant).
- `dialog`: A utility that helps to display dialog boxes from shell scripts, often used for text-based UI configuration tools like `nmtui`. See [ArchWiki: Dialog](https://wiki.archlinux.org/title/Dialog).


## Step 2: Verify WiFi Support

After reboot, you will use NetworkManager to connect to WiFi networks. Command-line tools like `nmcli` and text-based user interfaces like `nmtui` provide flexible ways to manage your wireless connections. For usage details, refer to the [ArchWiki on nmcli](https://wiki.archlinux.org/title/NetworkManager#nmcli) and [ArchWiki on nmtui](https://wiki.archlinux.org/title/NetworkManager#nmtui).

**Command line:**
```bash
nmcli device wifi list
nmcli device wifi connect "SSID" password "password"
```

**Text UI:**
```bash
nmtui
```

**GUI:**
- NetworkManager applet (if desktop environment installed)

---

**SUCCESS:** WiFi support installed

**Note:** WiFi configuration happens after first boot using NetworkManager

**Next:** Continue with other configuration modules

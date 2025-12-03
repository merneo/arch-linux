# Module: WiFi Configuration

**Purpose:** Install and configure WiFi support

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)
- NetworkManager enabled (module `11-networkmanager.md`)

**Time:** 2-3 minutes

---

## Step 1: Install WiFi Packages

```bash
pacman -S wireless_tools wpa_supplicant dialog
```

**Package breakdown:**
- `wireless_tools` - WiFi utilities
- `wpa_supplicant` - WPA/WPA2 authentication
- `dialog` - TUI dialogs (for WiFi setup)

---

## Step 2: Verify WiFi Support

**After reboot, use NetworkManager to connect:**

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

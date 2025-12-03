# Module: NetworkManager Setup

**Purpose:** Enable NetworkManager for network connectivity

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)
- NetworkManager installed (included in `00-core-installation.md`)

**Time:** 1 minute

---

## Enable NetworkManager Service

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
- Use `nmcli` or `nmtui` to configure WiFi
- Or use NetworkManager GUI applet

---

**Next:** Continue with other configuration modules

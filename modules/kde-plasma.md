# Module: KDE Plasma Desktop Environment

**Purpose:** Install and configure the KDE Plasma Desktop Environment. [KDE Plasma](https://wiki.archlinux.org/title/KDE) is a feature-rich and highly customizable desktop environment known for its modern aesthetics and powerful tools.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `user-creation.md`)
- Network connectivity (see `networkmanager.md`)

**Time:** 15-30 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Install KDE Plasma Packages

```bash
sudo pacman -S plasma kde-applications sddm
```

**Package breakdown:**
- `plasma`: The core [KDE Plasma desktop environment](https://wiki.archlinux.org/title/KDE).
- `kde-applications`: A meta-package containing many common [KDE applications](https://wiki.archlinux.org/title/KDE#Applications).
- `sddm`: [Simple Desktop Display Manager (SDDM)](https://wiki.archlinux.org/title/SDDM), which is the recommended display manager for KDE Plasma.

---

## Step 2: Enable Display Manager

[SDDM (Simple Desktop Display Manager)](https://wiki.archlinux.org/title/SDDM) is a modern display manager primarily designed for desktop environments using Qt, making it a natural fit for KDE Plasma. It provides a graphical login interface.

```bash
sudo systemctl enable sddm.service
```

---

## Step 3: Reboot

Reboot your system to start KDE Plasma.

```bash
reboot
```

After reboot, you should be greeted by the SDDM login screen. Log in with your user account.

---

**SUCCESS:** KDE Plasma Desktop Environment installed and configured.

---

## Troubleshooting

For more extensive troubleshooting on KDE Plasma, refer to the [ArchWiki on KDE#Troubleshooting](https://wiki.archlinux.org/title/KDE#Troubleshooting).

### Problem: SDDM login screen doesn't appear
**Solution:**
1. Check SDDM service: `systemctl status sddm.service`
2. Enable service: `systemctl enable --now sddm.service`
3. Check logs: `journalctl -u sddm.service -b`
4. Verify display server: Check if Xorg or Wayland is configured

### Problem: KDE Plasma session fails to start
**Solution:**
1. Check session logs: `journalctl --user -b -e`
2. Verify KDE is installed: `pacman -Q plasma`
3. Check for conflicting display managers: Disable other DMs
4. Try starting from TTY: `startx` or switch session type

### Problem: KDE applications don't work
**Solution:**
1. Verify Qt libraries: `pacman -Q qt5-base qt6-base`
2. Check environment variables: `echo $QT_QPA_PLATFORM`
3. Rebuild application cache: `sudo update-desktop-database`
4. Check KDE configuration: `~/.config/kdeglobals`

---

**Next:** Continue with other configuration modules or customize your KDE Plasma experience.

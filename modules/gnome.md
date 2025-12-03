# Module: GNOME Desktop Environment

**Purpose:** Install and configure the GNOME Desktop Environment. [GNOME](https://wiki.archlinux.org/title/GNOME) is a popular, user-friendly desktop environment known for its modern interface and integrated applications.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `user-creation.md`)
- Network connectivity (see `networkmanager.md`)

**Time:** 15-30 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Install GNOME Packages

```bash
sudo pacman -S gnome gnome-extra gnome-tweaks nautilus-share
```

**Package breakdown:**
- `gnome`: The core [GNOME](https://wiki.archlinux.org/title/GNOME) desktop environment meta-package.
- `gnome-extra`: Additional [GNOME applications](https://wiki.archlinux.org/title/GNOME#Applications) that complement the core desktop.
- `gnome-tweaks`: A utility for fine-tuning various GNOME settings not available through the standard settings panel.
- `nautilus-share`: Enables easy file sharing directly from the [Nautilus](https://wiki.archlinux.org/title/Nautilus) file manager.

---

## Step 2: Enable Display Manager

[GDM (GNOME Display Manager)](https://wiki.archlinux.org/title/GDM) is the default display manager for GNOME. A display manager provides a graphical login screen and manages user sessions.

```bash
sudo systemctl enable gdm.service
```

---

## Step 3: Reboot

Reboot your system to start GNOME.

```bash
reboot
```

After reboot, you should be greeted by the GDM login screen. Log in with your user account.

---

**SUCCESS:** GNOME Desktop Environment installed and configured.

---

## Troubleshooting

For more extensive troubleshooting on GNOME, refer to the [ArchWiki on GNOME#Troubleshooting](https://wiki.archlinux.org/title/GNOME#Troubleshooting).

### Problem: GDM login screen doesn't appear
**Solution:**
1. Check GDM service: `systemctl status gdm.service`
2. Enable service: `systemctl enable --now gdm.service`
3. Check logs: `journalctl -u gdm.service -b`
4. Verify display server: `echo $XDG_SESSION_TYPE` (should show `wayland` or `x11`)

### Problem: GNOME session fails to start
**Solution:**
1. Check session logs: `journalctl --user -b -e`
2. Verify GNOME is installed: `pacman -Q gnome`
3. Check for conflicting packages: Remove other desktop environments if installed
4. Try starting from TTY: `startx` or switch to Wayland session

### Problem: Applications don't appear in GNOME
**Solution:**
1. Rebuild application cache: `sudo update-desktop-database`
2. Restart GNOME Shell: Press Alt+F2, type `r`, press Enter
3. Check application files: `ls /usr/share/applications/`
4. Verify .desktop files: Check file permissions and content

---

**Next:** Continue with other configuration modules or customize your GNOME experience.

# Module: XFCE Desktop Environment

**Purpose:** Install and configure the XFCE Desktop Environment. [XFCE](https://wiki.archlinux.org/title/Xfce) is a lightweight, GTK-based desktop environment that aims to be fast and low on system resources while still being visually appealing and user-friendly.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `user-creation.md`)
- Network connectivity (see `networkmanager.md`)

**Time:** 10-20 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Install XFCE Packages

```bash
sudo pacman -S xfce4 xfce4-goodies lightdm lightdm-gtk-greeter
```

**Package breakdown:**
- `xfce4`: The core [XFCE desktop environment](https://wiki.archlinux.org/title/Xfce) meta-package.
- `xfce4-goodies`: A collection of useful plugins and applications that extend [XFCE's functionality](https://wiki.archlinux.org/title/Xfce#Applications).
- `lightdm`: [LightDM (Light Display Manager)](https://wiki.archlinux.org/title/LightDM), a fast and lightweight display manager commonly used with XFCE.
- `lightdm-gtk-greeter`: The GTK-based greeter (login screen) for LightDM.

---

## Step 2: Enable Display Manager

[LightDM (Light Display Manager)](https://wiki.archlinux.org/title/LightDM) is a popular and lightweight display manager often used with XFCE. It provides a graphical login screen.

```bash
sudo systemctl enable lightdm.service
```

---

## Step 3: Reboot

Reboot your system to start XFCE.

```bash
reboot
```

After reboot, you should be greeted by the LightDM login screen. Log in with your user account.

---

**SUCCESS:** XFCE Desktop Environment installed and configured.

---

## Troubleshooting

For more extensive troubleshooting on XFCE, refer to the [ArchWiki on Xfce#Troubleshooting](https://wiki.archlinux.org/title/Xfce#Troubleshooting).

### Problem: LightDM login screen doesn't appear
**Solution:**
1. Check LightDM service: `systemctl status lightdm.service`
2. Enable service: `systemctl enable --now lightdm.service`
3. Check logs: `journalctl -u lightdm.service -b`
4. Verify Xorg is installed: `pacman -Q xorg-server`

### Problem: XFCE session fails to start
**Solution:**
1. Check session logs: `journalctl --user -b -e`
2. Verify XFCE is installed: `pacman -Q xfce4`
3. Check Xorg logs: `cat ~/.local/share/xorg/Xorg.0.log`
4. Try starting manually: `startxfce4` from TTY

### Problem: XFCE panel or applications missing
**Solution:**
1. Reset XFCE configuration: `xfce4-panel -r` (restart panel)
2. Check application files: `ls /usr/share/applications/`
3. Rebuild cache: `sudo update-desktop-database`
4. Restore default settings: `xfce4-settings-manager`

---

**Next:** Continue with other configuration modules or customize your XFCE experience.

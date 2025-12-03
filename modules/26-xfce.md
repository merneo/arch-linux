# Module: XFCE Desktop Environment

**Purpose:** Install and configure the XFCE Desktop Environment. [XFCE](https://wiki.archlinux.org/title/Xfce) is a lightweight, GTK-based desktop environment that aims to be fast and low on system resources while still being visually appealing and user-friendly.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `04-user-creation.md`)
- Network connectivity (see `11-networkmanager.md`)

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

**Next:** Continue with other configuration modules or customize your XFCE experience.

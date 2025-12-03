# Module: GNOME Desktop Environment

**Purpose:** Install and configure the GNOME Desktop Environment. [GNOME](https://wiki.archlinux.org/title/GNOME) is a popular, user-friendly desktop environment known for its modern interface and integrated applications.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `04-user-creation.md`)
- Network connectivity (see `11-networkmanager.md`)

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

**Next:** Continue with other configuration modules or customize your GNOME experience.

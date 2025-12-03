# Module: GNOME Desktop Environment

**Purpose:** Install and configure the GNOME Desktop Environment

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
- `gnome`: The core GNOME desktop environment.
- `gnome-extra`: Additional GNOME applications (e.g., Maps, Weather, Boxes).
- `gnome-tweaks`: Utility for fine-tuning GNOME settings.
- `nautilus-share`: Enables file sharing through Nautilus.

---

## Step 2: Enable Display Manager

GNOME uses GDM (GNOME Display Manager) by default.

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

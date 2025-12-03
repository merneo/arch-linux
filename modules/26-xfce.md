# Module: XFCE Desktop Environment

**Purpose:** Install and configure the XFCE Desktop Environment

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
- `xfce4`: The core XFCE desktop environment.
- `xfce4-goodies`: A collection of useful plugins and applications for XFCE.
- `lightdm`: A lightweight display manager.
- `lightdm-gtk-greeter`: The GTK greeter for LightDM.

---

## Step 2: Enable Display Manager

XFCE typically uses LightDM (Light Display Manager).

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

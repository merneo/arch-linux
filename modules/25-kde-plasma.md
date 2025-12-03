# Module: KDE Plasma Desktop Environment

**Purpose:** Install and configure the KDE Plasma Desktop Environment

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `04-user-creation.md`)
- Network connectivity (see `11-networkmanager.md`)

**Time:** 15-30 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Install KDE Plasma Packages

```bash
sudo pacman -S plasma kde-applications sddm
```

**Package breakdown:**
- `plasma`: The core KDE Plasma desktop environment.
- `kde-applications`: A meta-package containing many common KDE applications.
- `sddm`: Simple Desktop Display Manager, recommended for KDE Plasma.

---

## Step 2: Enable Display Manager

KDE Plasma typically uses SDDM (Simple Desktop Display Manager).

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

**Next:** Continue with other configuration modules or customize your KDE Plasma experience.

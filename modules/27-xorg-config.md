# Module: Xorg Display Server Configuration

**Purpose:** Install and configure the Xorg Display Server, a foundational component for many desktop environments.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `04-user-creation.md`)
- Network connectivity (see `11-networkmanager.md`)
- (Optional) Desktop environment installed (e.g., GNOME, KDE, XFCE)

**Time:** 5-10 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Install Xorg Server and Utilities

```bash
sudo pacman -S xorg-server xorg-xinit xorg-apps xorg-drivers
```

**Package breakdown:**
- `xorg-server`: The Xorg display server itself.
- `xorg-xinit`: Tools for manually starting an Xorg session (e.g., `startx`).
- `xorg-apps`: A collection of basic Xorg applications and utilities.
- `xorg-drivers`: Common open-source display drivers. You may need specific proprietary drivers later (e.g., NVIDIA, AMDGPU-PRO).

---

## Step 2: Install Essential Xorg Fonts

```bash
sudo pacman -S xorg-fonts-misc ttf-dejavu ttf-liberation
```

**Package breakdown:**
- `xorg-fonts-misc`: Miscellaneous Xorg fonts.
- `ttf-dejavu`: A font family designed for good readability.
- `ttf-liberation`: Another common and well-hinted font family.

---

## Step 3: Configure Input Devices (Optional, but Recommended)

Install `xf86-input-libinput` for modern input device handling. If you have specific needs for other input drivers (e.g., Wacom tablets), install them separately.

```bash
sudo pacman -S xf86-input-libinput
```

---

## Step 4: Test Xorg Installation

You can try to start a very basic Xorg session.

```bash
startx
```

This should launch a simple X session with a terminal. To exit, type `exit` in the terminal.

---

**SUCCESS:** Xorg Display Server installed. You can now use display managers (GDM, SDDM, LightDM) or `startx` to launch graphical environments.

**Next:** Install a Desktop Environment (if not already done) or configure a Window Manager.

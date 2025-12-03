# Module: Xorg Display Server Configuration

**Purpose:** Install and configure the Xorg Display Server, a foundational component for many desktop environments and window managers on Linux. [Xorg](https://wiki.archlinux.org/title/Xorg) is the free and open-source implementation of the X Window System.

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
- `xorg-server`: The core [Xorg display server](https://wiki.archlinux.org/title/Xorg) itself, which manages graphical output and input devices.
- `xorg-xinit`: Provides the `startx` script, a basic way to manually start an [Xorg session](https://wiki.archlinux.org/title/Xinit).
- `xorg-apps`: A collection of basic [Xorg applications](https://wiki.archlinux.org/title/Xorg#Applications) and utilities (e.g., `xrandr` for display configuration).
- `xorg-drivers`: A meta-package that provides many common open-source display drivers for various graphics cards. Specific drivers like `xf86-video-intel`, `xf86-video-amdgpu`, etc., or proprietary drivers for NVIDIA, might be needed. Refer to [ArchWiki: Xorg#Driver_installation](https://wiki.archlinux.org/title/Xorg#Driver_installation) for details.

---

## Step 2: Install Essential Xorg Fonts

Proper font rendering is essential for a pleasant graphical experience. These packages provide common and widely used fonts. For a comprehensive guide on font configuration, refer to the [ArchWiki on Fonts](https://wiki.archlinux.org/title/Fonts).

```bash
sudo pacman -S xorg-fonts-misc ttf-dejavu ttf-liberation
```

**Package breakdown:**
- `xorg-fonts-misc`: Miscellaneous Xorg fonts, often used for basic system displays.
- `ttf-dejavu`: A highly readable font family, widely used in various operating systems.
- `ttf-liberation`: Another common and well-hinted font family, providing good rendering on different display types.

---

## Step 3: Configure Input Devices (Optional, but Recommended)

`libinput` is a library that handles input devices for Wayland compositors and provides a generic X.Org input driver. `xf86-input-libinput` is the Xorg driver that utilizes `libinput`. For more details, refer to the [ArchWiki on libinput](https://wiki.archlinux.org/title/Libinput).

Install `xf86-input-libinput` for modern input device handling. If you have specific needs for other input drivers (e.g., Wacom tablets), install them separately.

```bash
sudo pacman -S xf86-input-libinput
```

---

## Step 4: Test Xorg Installation

The `startx` command initiates an X session. This is a basic way to test if Xorg is correctly installed and can launch a graphical environment. For more information, refer to the [ArchWiki on Xinit](https://wiki.archlinux.org/title/Xinit).

You can try to start a very basic Xorg session.

```bash
startx
```

This should launch a simple X session with a terminal. To exit, type `exit` in the terminal.

---

**SUCCESS:** Xorg Display Server installed. You can now use display managers (GDM, SDDM, LightDM) or `startx` to launch graphical environments.

**Next:** Install a Desktop Environment (if not already done) or configure a Window Manager.

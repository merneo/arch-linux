# Module: Wayland Display Server Configuration

**Purpose:** Install and configure a basic Wayland display server setup, often through a compositor like [Sway](https://wiki.archlinux.org/title/Sway) or a desktop environment that uses Wayland (e.g., GNOME, KDE). [Wayland](https://wiki.archlinux.org/title/Wayland) is a modern display server protocol designed to be simpler and more secure than Xorg.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `04-user-creation.md`)
- Network connectivity (see `11-networkmanager.md`)
- (Optional) Desktop environment installed (e.g., GNOME or KDE already provides Wayland support)

**Time:** 5-15 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Install a Wayland Compositor (Example: Sway)

For a minimal Wayland setup, you typically install a compositor. [Sway](https://wiki.archlinux.org/title/Sway) is a popular tiling Wayland compositor that is a drop-in replacement for i3, built on `wlroots`.

```bash
sudo pacman -S sway swaylock swayidle dmenu foot
```

**Package breakdown (for Sway example):**
- `sway`: The [Wayland compositor](https://wiki.archlinux.org/title/Sway) itself.
- `swaylock`: A screen locker for Sway.
- `swayidle`: Idleness management for Sway, allowing actions based on user inactivity.
- `dmenu`: A dynamic menu, commonly used for launching applications in tiling window managers.
- `foot`: A fast, lightweight [Wayland terminal emulator](https://wiki.archlinux.org/title/Foot).

**Note:** If you are using [GNOME](https://wiki.archlinux.org/title/GNOME) or [KDE](https://wiki.archlinux.org/title/KDE), they come with their own Wayland compositors (Mutter for GNOME, KWin for KDE) and display managers that handle Wayland sessions automatically. You generally don't need to install a separate compositor like Sway in that case, but these instructions are for a more minimal setup or for users who prefer tiling window managers.

---

## Step 2: Install Essential Wayland Fonts

Proper font rendering is essential for a pleasant graphical experience across all display servers. For a comprehensive guide on font configuration in Arch Linux, refer to the [ArchWiki on Fonts](https://wiki.archlinux.org/title/Fonts).

```bash
sudo pacman -S ttf-dejavu ttf-liberation
```

**Package breakdown:**
- `ttf-dejavu`: A highly readable font family, widely used in various operating systems.
- `ttf-liberation`: Another common and well-hinted font family, providing good rendering on different display types.

---

## Step 3: Configure Input Devices

Wayland compositors handle input devices directly using `libinput`, a library that provides a generic input event handling. Ensuring `libinput` is installed (it often is as a dependency) is good practice. For more details, refer to the [ArchWiki on libinput](https://wiki.archlinux.org/title/Libinput).

```bash
sudo pacman -S libinput
```

---

## Step 4: Start a Wayland Session (Example: Sway)

To start a Wayland session, especially with a standalone compositor like Sway, you typically initiate it from a TTY. Desktop environments using Wayland (like GNOME or KDE) often provide session selections in their display managers. For comprehensive details on starting Wayland sessions, refer to the [ArchWiki on Wayland#Starting a Wayland session](https://wiki.archlinux.org/title/Wayland#Starting_a_Wayland_session).

To start Sway from a TTY, you would typically use `exec sway` in your `.bash_profile` or `startsway` in your `.bashrc`.

Alternatively, if you installed a display manager like GDM or SDDM (with GNOME or KDE), you can select the Wayland session from the login screen. For Sway with a display manager, you might need additional configuration or a specific login entry.

For a manual start from TTY:

1.  Switch to a TTY (Ctrl+Alt+F2 to F6).
2.  Log in as your user.
3.  Execute: `sway`

This should launch your Sway session.

---

**SUCCESS:** Basic Wayland display server (via Sway) installed and configured.

**Next:** Customize your Wayland compositor, install a display manager (if using Sway and want graphical login), or install a full desktop environment that uses Wayland.

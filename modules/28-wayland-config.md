# Module: Wayland Display Server Configuration

**Purpose:** Install and configure a basic Wayland display server setup, often through a compositor like Sway or a desktop environment that uses Wayland (e.g., GNOME, KDE).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `04-user-creation.md`)
- Network connectivity (see `11-networkmanager.md`)
- (Optional) Desktop environment installed (e.g., GNOME or KDE already provides Wayland support)

**Time:** 5-15 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Install a Wayland Compositor (Example: Sway)

For a minimal Wayland setup, you typically install a compositor. Sway is a popular tiling Wayland compositor that is a drop-in replacement for i3.

```bash
sudo pacman -S sway swaylock swayidle dmenu foot
```

**Package breakdown (for Sway example):**
- `sway`: The Wayland compositor itself.
- `swaylock`: Screen locker for Sway.
- `swayidle`: Idleness management for Sway.
- `dmenu`: A dynamic menu for launching applications (often used with Sway).
- `foot`: A fast, lightweight Wayland terminal emulator.

**Note:** If you are using GNOME or KDE, they come with their own Wayland compositors (Mutter for GNOME, KWin for KDE) and display managers that handle Wayland sessions automatically. You generally don't need to install a separate compositor like Sway in that case, but these instructions are for a more minimal setup or for users who prefer tiling window managers.

---

## Step 2: Install Essential Wayland Fonts

```bash
sudo pacman -S ttf-dejavu ttf-liberation
```

**Package breakdown:**
- `ttf-dejavu`: A font family designed for good readability.
- `ttf-liberation`: Another common and well-hinted font family.

---

## Step 3: Configure Input Devices

Wayland compositors typically handle input devices automatically, but ensuring `libinput` is installed is good practice.

```bash
sudo pacman -S libinput
```

---

## Step 4: Start a Wayland Session (Example: Sway)

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

# Module: Hyprland Window Manager

**Purpose:** Install and configure `Hyprland`, a dynamic tiling Wayland compositor, for a modern, fluid, and highly customizable desktop experience.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `user-creation.md`)
- Wayland Display Server is implicit as Hyprland is a Wayland compositor.
- Network connectivity (see `networkmanager.md`)
- (Optional but Recommended) A terminal emulator like `foot` or `kitty`.

**Time:** 15-30 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## What is Hyprland?

[Hyprland](https://wiki.archlinux.org/title/Hyprland) is a dynamic tiling Wayland compositor built on [`wlroots`](https://gitlab.freedesktop.org/wlroots/wlroots), designed to offer a fluid and highly customizable experience with eye-candy animations. It combines the efficiency of a tiling window manager with the modern features and security benefits of the [Wayland protocol](https://wiki.archlinux.org/title/Wayland). It's an excellent choice for users seeking a powerful and visually appealing Wayland-native desktop.

---

## Step 1: Install Hyprland and Essential Components

```bash
sudo pacman -S hyprland kitty waybar wofi grim slurp
```

**Package breakdown:**
- `hyprland`: The core [Hyprland Wayland compositor](https://wiki.archlinux.org/title/Hyprland).
- `kitty`: A fast, feature-rich, GPU-accelerated [terminal emulator](https://wiki.archlinux.org/title/Kitty) that works well on Wayland.
- `waybar`: A highly customizable [Wayland bar](https://wiki.archlinux.org/title/Waybar) for status display.
- `wofi`: A [launcher for Wayland](https://wiki.archlinux.org/title/Wofi) (dmenu-like) for running applications.
- `grim`: A utility for [screenshotting Wayland](https://wiki.archlinux.org/title/Take_screenshot#Wayland) desktops.
- `slurp`: A tool to select a region for `grim`.

---

## Step 2: Configure Hyprland

Hyprland's configuration is done via a single file, typically located at `~/.config/hypr/hyprland.conf`. The default configuration provides a functional setup, but extensive customization is possible.

```bash
# Copy default config (if not already present)
mkdir -p ~/.config/hypr
cp /etc/xdg/hypr/hyprland.conf ~/.config/hypr/hyprland.conf

# Edit Hyprland configuration file
nano ~/.config/hypr/hyprland.conf
```

**Key configuration areas include:**
*   **`monitor` settings:** Define resolution, refresh rate, and position for each display.
*   **`input` settings:** Configure keyboard, mouse, touchpad behavior.
*   **`bind` rules:** Define keyboard shortcuts for launching applications, managing windows, and switching workspaces.
*   **`windowrule`s:** Apply specific rules to windows (e.g., floating, sticky).
*   **`exec` commands:** Autostart applications or services.

---

## Step 3: Start Hyprland Session

After installation, you can start a Hyprland session from your display manager (e.g., GDM, SDDM, LightDM) by selecting "Hyprland" from the session selector. Ensure your display manager is configured to support Wayland sessions.

Alternatively, you can start Hyprland manually from a TTY (though this setup is more common for i3/Sway, it can be adapted):

```bash
# Typically, display managers handle starting Wayland compositors.
# If starting manually from TTY, you might use a command like:
# Hyprland # (after having configured ~/.profile or ~/.bashrc to launch it)
```

---

## Troubleshooting

For more extensive troubleshooting on Hyprland, refer to the [ArchWiki on Hyprland#Troubleshooting](https://wiki.archlinux.org/title/Hyprland#Troubleshooting) or the [official Hyprland Wiki](https://wiki.hyprland.org/Hyprland-Setup/Troubleshooting/).

### Problem: Hyprland not starting
**Solution:**
1.  **Check Wayland support:** Ensure your GPU drivers support Wayland.
2.  **Verify configuration:** Check `~/.config/hypr/hyprland.conf` for syntax errors. `hyprctl reload` can sometimes show errors.
3.  **Check logs:** `journalctl --user -b -e` (for user systemd services) or `Hyprland` output from TTY can provide diagnostics.

### Problem: Applications not launching or behaving correctly
**Solution:**
1.  **Environment variables:** Ensure necessary Wayland environment variables are set (e.g., `XDG_SESSION_TYPE=wayland`, `MOZ_ENABLE_WAYLAND=1` for Firefox).
2.  **XWayland:** Some applications still require XWayland. Ensure it's installed (`xorg-xwayland`).

---

**SUCCESS:** Hyprland Window Manager installed and configured.

**Official Resources:**
- [ArchWiki: Hyprland](https://wiki.archlinux.org/title/Hyprland)
- [Hyprland Wiki](https://wiki.hyprland.org/)
- [Hyprland GitHub Repository](https://github.com/hyprwm/Hyprland)

**Next:** Customize your Hyprland configuration (`~/.config/hypr/hyprland.conf`) and explore the Hyprland Wiki for plugins and further customization.

# Module: i3 Window Manager

**Purpose:** Install and configure `i3`, a popular tiling window manager, for efficient keyboard-driven workflow and resource efficiency.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `user-creation.md`)
- Xorg Display Server installed (see `xorg-config.md`)
- Network connectivity (see `networkmanager.md`)

**Time:** 10-20 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## What is i3?

[i3 (i3wm)](https://wiki.archlinux.org/title/I3) is a tiling window manager for X11, designed primarily for advanced users and developers who prefer a keyboard-driven workflow. Unlike traditional floating window managers or desktop environments, i3 arranges windows into a non-overlapping grid, maximizing screen real estate and eliminating the need for mouse interaction for window management. Its configuration is based on plain text files, making it highly customizable.

---

## Step 1: Install i3 and Essential Components

```bash
sudo pacman -S i3-wm i3status dmenu i3lock xorg-xrandr
```

**Package breakdown:**
- `i3-wm`: The core i3 tiling window manager.
- `i3status`: A compact status bar program that displays system information (e.g., CPU load, memory, battery, time).
- `dmenu`: A fast, lightweight dynamic menu, often used to launch applications in i3.
- `i3lock`: A simple screen locker for i3.
- `xorg-xrandr`: Essential for display management, especially for multi-monitor setups.

---

## Step 2: Configure i3

Upon first login to an i3 session, you will be prompted to generate a configuration file.

### First-time Configuration:

1.  **Default config:** When i3 starts for the first time, it will ask if you want to generate a default configuration file. Choose **"Yes"**.
2.  **Mod key:** It will then ask you to choose a "Mod key" (modifier key) for keyboard shortcuts. `Alt` (Mod1) or `Windows key` (Mod4) are common choices. `Windows key` (Mod4) is generally recommended to avoid conflicts with application shortcuts.
3.  **Automatic launch:** i3 will automatically restart, and you will see your first i3 desktop.

The default configuration file will be created at `~/.config/i3/config`. You can edit this file to customize keybindings, workspace behavior, status bar, and more.

```bash
# Edit i3 configuration file
nano ~/.config/i3/config
```

---

## Step 3: Start i3 Session

After installation, you can start an i3 session from your display manager (e.g., GDM, SDDM, LightDM) by selecting "i3" or "i3 (with extras)" from the session selector.

If you are not using a display manager, you can start i3 manually from a TTY using `startx`:

```bash
# Add this to your ~/.xinitrc file
echo "exec i3" >> ~/.xinitrc

# Then start X from TTY
startx
```

---

## Troubleshooting

For more extensive troubleshooting on i3, refer to the [ArchWiki on i3#Troubleshooting](https://wiki.archlinux.org/title/I3#Troubleshooting).

### Problem: i3 not starting
**Solution:**
1. **Check Xorg installation:** Ensure Xorg is installed and functional.
2. **Verify `.xinitrc`:** If using `startx`, ensure `exec i3` is the last line in `~/.xinitrc`.
3. **Check display manager logs:** `journalctl -u <display_manager_service>` (e.g., `gdm.service`, `sddm.service`).
4. **i3 log:** `cat ~/.local/share/i3/i3.log`

---

**SUCCESS:** i3 Window Manager installed and configured.

**Official Resources:**
- [ArchWiki: i3](https://wiki.archlinux.org/title/I3)
- [i3 User's Guide](https://i3wm.org/docs/userguide.html)
- [i3 GitHub Repository](https://github.com/i3/i3)

**Next:** Customize your `i3` configuration (`~/.config/i3/config`) and explore the `i3` user's guide.

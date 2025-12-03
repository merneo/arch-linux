# Module: Laptop Touchpad Configuration

**Purpose:** Configure touchpad/trackpad for laptops (tap-to-click, gestures, scrolling). The touchpad is a crucial input device for laptops, and proper configuration greatly enhances usability. For comprehensive details on touchpad configuration in Arch Linux, refer to the [ArchWiki on Touchpad Synaptics](https://wiki.archlinux.org/title/Touchpad_Synaptics) and [ArchWiki on libinput](https://wiki.archlinux.org/title/Libinput).

**Prerequisites:**
- Inside chroot environment (module `chroot.md`) OR after first boot
- Laptop hardware (not desktop)
- Window manager or desktop environment installed (for GUI configuration)

**Time:** 5-10 minutes

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot

**Note:** This module is for **LAPTOPS only**. Desktop computers use external mice and don't have touchpads.

---

## Step 1: Verify Touchpad Detection

Verifying that your system correctly detects the touchpad is the first step. `libinput` is the standard Wayland and X.Org input driver, while `xinput` is an X Window System utility for configuring and testing input devices.

```bash
# List input devices. For libinput details, see [ArchWiki: libinput](https://wiki.archlinux.org/title/Libinput).
libinput list-devices | grep -i touchpad

# Or check with xinput (if X11 is installed). For xinput details, see [ArchWiki: xinput](https://wiki.archlinux.org/title/Xorg#Input_configuration).
xinput list | grep -i touchpad

# Expected output shows touchpad device
```

---

## Step 2: Install Required Packages

These packages provide the necessary drivers and utilities for managing input devices, particularly touchpads.

```bash
# libinput is usually included with desktop environments. For standalone libinput setup, see [ArchWiki: libinput](https://wiki.archlinux.org/title/Libinput).
# For minimal setups, install explicitly:
pacman -S libinput xf86-input-libinput

# xf86-input-libinput is the Xorg input driver based on libinput.
# For X11 environments:
pacman -S xorg-xinput # Provides the 'xinput' command for managing input devices. See [ArchWiki: xinput](https://wiki.archlinux.org/title/Xorg#Input_configuration).

# For Wayland (Hyprland, Sway, etc.):
# libinput is built-in, no additional packages needed beyond the compositor itself.
# See [ArchWiki: Wayland](https://wiki.archlinux.org/title/Wayland).
```

---

## Step 3: Configure Touchpad (Wayland - Hyprland)

[Hyprland](https://wiki.archlinux.org/title/Hyprland) is a dynamic tiling Wayland compositor based on `wlroots`. Touchpad configuration for Hyprland is typically done through its configuration file.

Edit `~/.config/hypr/hyprland.conf`:

```ini
input {
    touchpad {
        # Scroll direction: false = Unix (drag down = scroll down)
        natural_scroll = false

        # Tap-to-click: Single tap = left click
        tap-to-click = true

        # Disable touchpad while typing to prevent accidental touches
        disable_while_typing = true

        # Click behavior: clickfinger = 1-finger click = left, 2-finger = right
        clickfinger_behavior = true

        # Middle mouse button emulation (3-finger tap)
        middle_button_emulation = true

        # Tap-and-drag: Double tap and hold to drag
        tap-and-drag = true
    }
}
```

**Reload Hyprland:**
```bash
hyprctl reload
```

---

## Step 4: Configure Touchpad (X11)

For X11 environments, touchpad configuration can be done either temporarily via `xinput` commands or permanently through Xorg configuration files.

### Option 1: Using xinput (temporary, until reboot)

The `xinput` utility is used to configure X.Org input devices. Changes made with `xinput` are session-specific and will reset on reboot. For more details, refer to the [ArchWiki on xinput](https://wiki.archlinux.org/title/Xorg#Input_configuration).

```bash
# List touchpad device
xinput list

# Find touchpad ID (e.g., "AlpsPS/2 ALPS GlidePoint" id=12)

# Enable tap-to-click
xinput set-prop <TOUCHPAD_ID> "libinput Tapping Enabled" 1

# Enable natural scrolling (optional)
xinput set-prop <TOUCHPAD_ID> "libinput Natural Scrolling Enabled" 1

# Disable while typing
xinput set-prop <TOUCHPAD_ID> "libinput Disable While Typing Enabled" 1
```

### Option 2: Using Xorg configuration (permanent)

For persistent configuration across reboots, Xorg configuration files in `/etc/X11/xorg.conf.d/` are used. These files define how input devices are handled by the X server. Refer to the [ArchWiki on Xorg configuration](https://wiki.archlinux.org/title/Xorg#Input_configuration) for comprehensive details.

Create `/etc/X11/xorg.conf.d/40-libinput.conf`:

```bash
sudo nano /etc/X11/xorg.conf.d/40-libinput.conf
```

Add:
```
Section "InputClass"
    Identifier "libinput touchpad catchall"
    MatchIsTouchpad "on"
    MatchDevicePath "/dev/input/event*"
    Driver "libinput"
    Option "Tapping" "on"
    Option "NaturalScrolling" "false"
    Option "DisableWhileTyping" "on"
    Option "ClickMethod" "clickfinger"
EndSection
```

**Restart X server** (logout/login or reboot)

---

## Step 5: Verify Configuration

```bash
# Test tap-to-click
# Tap on touchpad - should register as left click

# Test two-finger scroll
# Use two fingers to scroll - should work vertically and horizontally

# Test right-click
# Two-finger tap - should register as right click

# Test gestures (if supported)
# Three-finger swipe - should work if configured
```

---

## Troubleshooting

For more extensive troubleshooting on touchpad issues, refer to the [ArchWiki on Touchpad Synaptics#Troubleshooting](https://wiki.archlinux.org/title/Touchpad_Synaptics#Troubleshooting) and [ArchWiki on libinput#Troubleshooting](https://wiki.archlinux.org/title/Libinput#Troubleshooting).

### Problem: Touchpad not detected
**Solution:**
1. Check if touchpad is enabled in BIOS/UEFI
2. Verify kernel module: `lsmod | grep psmouse`. For `lsmod` details, see [ArchWiki: Kernel modules](https://wiki.archlinux.org/title/Kernel_modules).
3. Check kernel messages: `dmesg | grep -i touchpad`. For `dmesg` details, see [ArchWiki: dmesg](https://wiki.archlinux.org/title/Dmesg).

### Problem: Tap-to-click not working
**Solution:**
1. Verify libinput is installed: `pacman -Q libinput`
2. Check configuration file syntax
3. For X11: Verify xorg.conf.d file exists and is correct

### Problem: Touchpad too sensitive/not sensitive enough
**Solution:**
- Adjust sensitivity in window manager settings
- For libinput: Use `libinput measure touchpad-size` to calibrate

---

**SUCCESS:** Touchpad configured and working

**Official Resources:**
- [ArchWiki: Touchpad Synaptics](https://wiki.archlinux.org/title/Touchpad_Synaptics)
- [ArchWiki: libinput](https://wiki.archlinux.org/title/Libinput)
- [Hyprland: Input Configuration](https://wiki.hyprland.org/Configuring/Variables/#input)

**Next:** Continue with other configuration modules

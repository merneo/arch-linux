# Module: Laptop Touchpad Configuration

**Purpose:** Configure touchpad/trackpad for laptops (tap-to-click, gestures, scrolling)

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`) OR after first boot
- Laptop hardware (not desktop)
- Window manager or desktop environment installed (for GUI configuration)

**Time:** 5-10 minutes

**Note:** This module is for **LAPTOPS only**. Desktop computers use external mice and don't have touchpads.

---

## Step 1: Verify Touchpad Detection

```bash
# List input devices
libinput list-devices | grep -i touchpad

# Or check with xinput (if X11 is installed)
xinput list | grep -i touchpad

# Expected output shows touchpad device
```

---

## Step 2: Install Required Packages

```bash
# libinput is usually included with desktop environments
# For minimal setups, install explicitly:
pacman -S libinput xf86-input-libinput

# For X11 environments:
pacman -S xorg-xinput

# For Wayland (Hyprland, Sway, etc.):
# libinput is built-in, no additional packages needed
```

---

## Step 3: Configure Touchpad (Wayland - Hyprland)

**For Hyprland window manager:**

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

**For X11 environments (GNOME, KDE, XFCE, etc.):**

### Option 1: Using xinput (temporary, until reboot)

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

### Problem: Touchpad not detected
**Solution:**
1. Check if touchpad is enabled in BIOS/UEFI
2. Verify kernel module: `lsmod | grep psmouse`
3. Check dmesg: `dmesg | grep -i touchpad`

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

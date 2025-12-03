# Step: Laptop Touchpad Configuration

**Purpose:** Configure touchpad/trackpad for laptops (tap-to-click, gestures, scrolling). For comprehensive details, refer to the [ArchWiki on Touchpad Synaptics](https://wiki.archlinux.org/title/Touchpad_Synaptics) and [ArchWiki on libinput](https://wiki.archlinux.org/title/Libinput).

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** Laptop hardware, window manager installed

## Commands

```bash
# Verify touchpad detection. See [ArchWiki: libinput](https://wiki.archlinux.org/title/Libinput) and [ArchWiki: Xorg#Input_configuration](https://wiki.archlinux.org/title/Xorg#Input_configuration).
libinput list-devices | grep -i touchpad

# Install packages (if needed). See [ArchWiki: libinput](https://wiki.archlinux.org/title/Libinput) and [ArchWiki: xinput](https://wiki.archlinux.org/title/Xorg#Input_configuration).
pacman -S libinput xf86-input-libinput
# For X11: pacman -S xorg-xinput

# Configure for Hyprland (Wayland). See [ArchWiki: Hyprland](https://wiki.archlinux.org/title/Hyprland).
# Edit ~/.config/hypr/hyprland.conf:
# input {
#     touchpad {
#         natural_scroll = false
#         tap-to-click = true
#         disable_while_typing = true
#         clickfinger_behavior = true
#         middle_button_emulation = true
#         tap-and-drag = true
#     }
# }
# Reload: hyprctl reload
```

**NEXT:** `webcam.md` or continue with other modules

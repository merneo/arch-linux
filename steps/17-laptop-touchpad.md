# Step: Laptop Touchpad Configuration

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** Laptop hardware, window manager installed

## Commands

```bash
# Verify touchpad detection
libinput list-devices | grep -i touchpad

# Install packages (if needed)
pacman -S libinput xf86-input-libinput
# For X11: pacman -S xorg-xinput

# Configure for Hyprland (Wayland)
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

**NEXT:** `18-laptop-webcam.md` or continue with other modules

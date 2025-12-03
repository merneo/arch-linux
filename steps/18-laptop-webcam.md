# Step: Laptop Webcam Configuration

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** Laptop hardware, webcam present

## Commands

```bash
# Verify webcam detection
lsusb | grep -i camera
ls -la /dev/video*

# Verify UVC driver
lsmod | grep uvcvideo

# Install packages (if needed)
pacman -S linux-firmware v4l-utils
# Optional GUI: pacman -S cheese  # or kamoso

# Test webcam
v4l2-ctl --list-devices
# Use cheese or kamoso to test
```

**NEXT:** `19-laptop-ir-camera.md` (if IR camera present) or `20-laptop-fingerprint.md` or continue

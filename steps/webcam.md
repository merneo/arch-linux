# Step: Laptop Webcam Configuration

**Purpose:** Configure built-in webcam for laptops. For comprehensive setup information, consult the [ArchWiki on Webcam setup](https://wiki.archlinux.org/title/Webcam_setup).

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** Laptop hardware, webcam present

## Commands

```bash
# Verify webcam detection. See [ArchWiki on lsusb](https://wiki.archlinux.org/title/USB#lsusb) and /dev/video* devices.
lsusb | grep -i camera
ls -la /dev/video*

# Verify UVC driver. See [ArchWiki: Kernel modules](https://wiki.archlinux.org/title/Kernel_modules).
lsmod | grep uvcvideo

# Install packages (if needed). For v4l-utils details, see [ArchWiki: V4l-utils](https://wiki.archlinux.org/title/Webcam_setup#V4l-utils).
pacman -S linux-firmware v4l-utils
# Optional GUI: pacman -S cheese  # [ArchWiki: Cheese](https://wiki.archlinux.org/title/Cheese) or [ArchWiki: Kamoso](https://wiki.archlinux.org/title/KDE#Applications)
# or kamoso

# Test webcam. Use `v4l2-ctl` from v4l-utils to list detected video devices.
# See [ArchWiki: V4l-utils](https://wiki.archlinux.org/title/Webcam_setup#V4l-utils).
v4l2-ctl --list-devices
# Use cheese or kamoso to test
```

**NEXT:** `ir-camera.md` (if IR camera present) or `fingerprint.md` or continue

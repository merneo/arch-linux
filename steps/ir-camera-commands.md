# Step: Laptop IR Camera Configuration

**Purpose:** Configure IR (Infrared) camera for face recognition (Howdy). For detailed information, refer to the [ArchWiki on Howdy](https://wiki.archlinux.org/title/Howdy).

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** Laptop hardware, IR camera present

## Commands

```bash
# Verify IR camera detection. See [ArchWiki on lsusb](https://wiki.archlinux.org/title/USB#lsusb) and /dev/video* devices.
lsusb | grep -i "ir\|camera"
ls -la /dev/video*

# Install Howdy (from AUR). Requires an [AUR helper](https://wiki.archlinux.org/title/AUR_helpers) like yay.
yay -S howdy-bin
# Or: yay -S howdy

# Configure Howdy. Edit the configuration file for camera path.
# See [Howdy configuration](https://github.com/boltgolt/howdy/wiki/Configuration).
sudo nano /lib/security/howdy/config.ini
# Set camera_path to IR camera (e.g., /dev/video2)
# Set device_path to IR camera device

# Enable Howdy service. See [ArchWiki: systemd](https://wiki.archlinux.org/title/Systemd).
sudo systemctl enable --now python3-validity

# Enroll face. See [ArchWiki: Howdy#Enrollment](https://wiki.archlinux.org/title/Howdy#Enrollment).
sudo howdy add
```

**NEXT:** `fingerprint.md` or continue

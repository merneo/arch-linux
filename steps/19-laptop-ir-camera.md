# Step: Laptop IR Camera Configuration

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** Laptop hardware, IR camera present

## Commands

```bash
# Verify IR camera detection
lsusb | grep -i "ir\|camera"
ls -la /dev/video*

# Install Howdy (from AUR)
yay -S howdy-bin
# Or: yay -S howdy

# Configure Howdy
sudo nano /lib/security/howdy/config.ini
# Set camera_path to IR camera (e.g., /dev/video2)
# Set device_path to IR camera device

# Enable Howdy service
sudo systemctl enable --now python3-validity

# Enroll face
sudo howdy add
```

**NEXT:** `20-laptop-fingerprint.md` or continue

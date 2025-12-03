# Step: Laptop Fingerprint Reader Configuration

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** Laptop hardware, fingerprint reader present

## Commands

```bash
# Verify fingerprint reader detection
lsusb | grep -i "fingerprint\|validity"

# For Validity Sensors (most common):
yay -S python-validity-git
pacman -S fprintd

# For other models:
pacman -S fprintd

# Enable services
sudo systemctl enable --now python3-validity  # For Validity Sensors
sudo systemctl enable --now fprintd

# Enroll fingerprint
fprintd-enroll
# Or: sudo pam_fprintd_enroll

# Verify
fprintd-list
```

**NEXT:** Continue with other modules

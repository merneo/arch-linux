# Step: Laptop Fingerprint Reader Configuration

**Purpose:** Configure fingerprint reader for laptops. For comprehensive information, refer to the [ArchWiki on Fprint](https://wiki.archlinux.org/title/Fprint).

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** Laptop hardware, fingerprint reader present

## Commands

```bash
# Verify fingerprint reader detection. See [ArchWiki on lsusb](https://wiki.archlinux.org/title/USB#lsusb).
lsusb | grep -i "fingerprint\|validity"

# For Validity Sensors (most common):
# Install from AUR. Requires an [AUR helper](https://wiki.archlinux.org/title/AUR_helpers) like yay.
yay -S python-validity-git
# Install fprintd (fingerprint daemon). See [ArchWiki: Fprint](https://wiki.archlinux.org/title/Fprint).
pacman -S fprintd

# For other models:
pacman -S fprintd

# Enable services. See [ArchWiki: systemd](https://wiki.archlinux.org/title/Systemd).
sudo systemctl enable --now python3-validity  # For Validity Sensors
sudo systemctl enable --now fprintd

# Enroll fingerprint. See [ArchWiki: Fprint#Enrollment](https://wiki.archlinux.org/title/Fprint#Enrollment).
fprintd-enroll
# Or: sudo pam_fprintd_enroll

# Verify. See [ArchWiki: Fprint#Verification](https://wiki.archlinux.org/title/Fprint#Verification).
fprintd-list
```

**NEXT:** Continue with other modules

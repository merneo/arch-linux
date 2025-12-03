# Step: UFW Firewall Configuration

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** SSH server configured (if using SSH)

## Commands

```bash
# Install UFW
pacman -S ufw

# Set default policies
ufw default deny incoming
ufw default allow outgoing

# Allow SSH on port 1991
ufw allow 1991/tcp

# Enable firewall
ufw enable
# Confirm: y
```

## Verification

```bash
ufw status verbose
ufw status | grep 1991
```

**NEXT:** `23-fail2ban.md`

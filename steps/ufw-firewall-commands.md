# Step: UFW Firewall Configuration

**Purpose:** Install and configure UFW (Uncomplicated Firewall) with SSH access. UFW simplifies firewall management. For a comprehensive guide, refer to the [ArchWiki on Uncomplicated Firewall (UFW)](https://wiki.archlinux.org/title/Uncomplicated_Firewall).

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** SSH server configured (if using SSH)

## Commands

```bash
# Install UFW. The 'ufw' package provides the Uncomplicated Firewall utility.
pacman -S ufw

# Set default policies. By default, incoming traffic is denied and outgoing is allowed.
# See [ArchWiki: UFW#Configuration](https://wiki.archlinux.org/title/Uncomplicated_Firewall#Configuration).
ufw default deny incoming
ufw default allow outgoing

# Allow SSH on port 1991. This creates a rule to permit incoming SSH connections.
# See [ArchWiki: UFW#Rules](https://wiki.archlinux.org/title/Uncomplicated_Firewall#Rules).
ufw allow 1991/tcp

# Enable firewall. This activates the firewall and starts enforcing its rules.
# See [ArchWiki: UFW#Enable/Disable](https://wiki.archlinux.org/title/Uncomplicated_Firewall#Enable/Disable).
ufw enable
# Confirm: y
```

## Verification

```bash
ufw status verbose # Check detailed UFW status. See [ArchWiki: UFW#Status](https://wiki.archlinux.org/title/Uncomplicated_Firewall#Status).
ufw status | grep 1991 # Verify SSH port is allowed.
```

**NEXT:** `fail2ban.md`

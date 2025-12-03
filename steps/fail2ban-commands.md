# Step: Fail2ban Configuration for SSH

**Purpose:** Install and configure fail2ban to prevent SSH brute force attacks. For comprehensive information, refer to the [ArchWiki on Fail2ban](https://wiki.archlinux.org/title/Fail2ban).

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** SSH server configured, UFW firewall configured

## Commands

```bash
# Install fail2ban. The 'fail2ban' package provides the daemon and configuration files.
pacman -S fail2ban

# Create configuration. Configures fail2ban to use the systemd journal for log monitoring.
# See [ArchWiki: systemd/Journal](https://wiki.archlinux.org/title/Systemd/Journal).
mkdir -p /etc/fail2ban/jail.d
cat > /etc/fail2ban/jail.d/sshd.conf << 'EOF'
[sshd]
enabled = true
port = 1991
filter = sshd
backend = systemd
journalmatch = _SYSTEMD_UNIT=sshd.service
maxretry = 3
bantime = 3600
findtime = 600
EOF

# Enable and start fail2ban. See [ArchWiki: systemd](https://wiki.archlinux.org/title/Systemd).
systemctl enable fail2ban
systemctl start fail2ban
```

## Verification

```bash
systemctl is-active fail2ban # Verify fail2ban service is active. See [ArchWiki: systemd](https://wiki.archlinux.org/title/Systemd).
fail2ban-client status sshd # Verify SSH jail status. See [ArchWiki: Fail2ban](https://wiki.archlinux.org/title/Fail2ban).
```

**NEXT:** Continue with other modules

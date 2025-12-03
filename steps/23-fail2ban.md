# Step: Fail2ban Configuration for SSH

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** SSH server configured, UFW firewall configured

## Commands

```bash
# Install fail2ban
pacman -S fail2ban

# Create configuration
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

# Enable and start fail2ban
systemctl enable fail2ban
systemctl start fail2ban
```

## Verification

```bash
systemctl is-active fail2ban
fail2ban-client status sshd
```

**NEXT:** Continue with other modules

# Step: SSH Server Configuration

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** Network connection established

## Commands

```bash
# Install SSH server
pacman -S openssh

# Configure SSH
nano /etc/ssh/sshd_config
# Set: Port 1991
# Set: PermitRootLogin no
# Set: PasswordAuthentication yes
# Optional: MaxAuthTries 3, LoginGraceTime 60

# Enable SSH service
systemctl enable sshd
# If after first boot: systemctl start sshd
```

## Verification

```bash
systemctl is-active sshd
ss -tlnp | grep :1991
grep "PermitRootLogin" /etc/ssh/sshd_config
grep "^Port" /etc/ssh/sshd_config
```

**NEXT:** `22-ufw-firewall.md`

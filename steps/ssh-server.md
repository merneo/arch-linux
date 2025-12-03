# Step: SSH Server Configuration

**Purpose:** Install and configure SSH (Secure Shell) server with custom port and security settings. For comprehensive information, refer to the [ArchWiki on OpenSSH](https://wiki.archlinux.org/title/OpenSSH).

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot
**PREREQUISITES:** Network connection established

## Commands

```bash
# Install SSH server. See [ArchWiki: OpenSSH#Installation](https://wiki.archlinux.org/title/OpenSSH#Installation).
pacman -S openssh

# Configure SSH. Edit the main configuration file '/etc/ssh/sshd_config'.
# For detailed options, consult 'man sshd_config' or [ArchWiki: OpenSSH/Configuration](https://wiki.archlinux.org/title/OpenSSH#Configuration).
nano /etc/ssh/sshd_config
# Set: Port 1991 (Custom port for security)
# Set: PermitRootLogin no (Security best practice)
# Set: PasswordAuthentication yes (Can be disabled later for key-only)
# Optional: MaxAuthTries 3, LoginGraceTime 60

# Enable SSH service. See [ArchWiki: systemd](https://wiki.archlinux.org/title/Systemd).
systemctl enable sshd
# If after first boot: systemctl start sshd
```

## Verification

```bash
systemctl is-active sshd # Check if SSH service is active. See [ArchWiki: systemd](https://wiki.archlinux.org/title/Systemd).
ss -tlnp | grep :1991 # Verify SSH is listening on port 1991. Consult 'man ss'.
grep "PermitRootLogin" /etc/ssh/sshd_config # Verify root login setting.
grep "^Port" /etc/ssh/sshd_config # Verify configured port.
```

**NEXT:** `ufw-firewall.md`

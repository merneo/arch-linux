# Helper: SSH Server Setup with Security

**Purpose:** Quick reference for setting up SSH server with custom port, firewall, and fail2ban

**Repository:** https://github.com/merneo/arch-linux

---

## Quick Setup

### 1. Install and Configure SSH Server

```bash
# Install SSH server
pacman -S openssh

# Configure SSH
sudo nano /etc/ssh/sshd_config
```

**Key settings:**
- `Port 1991` - Custom SSH port
- `PermitRootLogin no` - Disable root login
- `PasswordAuthentication yes` - Allow password auth

**Enable and start:**
```bash
sudo systemctl enable --now sshd
```

### 2. Configure UFW Firewall

```bash
# Install UFW
pacman -S ufw

# Set defaults
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH on port 1991
sudo ufw allow 1991/tcp

# Enable firewall
sudo ufw enable
```

### 3. Install and Configure Fail2ban

```bash
# Install fail2ban
pacman -S fail2ban

# Create SSH jail config
sudo tee /etc/fail2ban/jail.d/sshd.conf << 'EOF'
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

# Enable and start
sudo systemctl enable --now fail2ban
```

---

## Verification

```bash
# Check SSH is running on port 1991
ss -tlnp | grep :1991

# Check UFW status
sudo ufw status

# Check fail2ban status
sudo fail2ban-client status sshd
```

---

## Security Summary

- **SSH Port:** 1991 (custom, reduces automated attacks)
- **Root Login:** Disabled (use sudo instead)
- **Firewall:** UFW - all outgoing allowed, only SSH (1991) incoming
- **Fail2ban:** Bans IPs after 3 failed login attempts for 1 hour

---

**Full Documentation:** See modules `21-ssh-server.md`, `22-ufw-firewall.md`, `23-fail2ban.md`

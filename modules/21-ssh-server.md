# Module: SSH Server Configuration

**Purpose:** Install and configure SSH server with custom port and security settings

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`) OR after first boot
- Network connection established

**Time:** 5-10 minutes

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot

**Note:** This module configures SSH server with:
- Custom port: **1991**
- Root login: **DISABLED** (security best practice)
- Password authentication: **ENABLED** (can be disabled later for key-only)

---

## Step 1: Install SSH Server

```bash
pacman -S openssh
```

---

## Step 2: Configure SSH Server

```bash
# Edit SSH server configuration
nano /etc/ssh/sshd_config
```

**Find and modify these lines:**

```bash
# Change SSH port from 22 to 1991
Port 1991

# Disable root login (security best practice)
PermitRootLogin no

# Allow password authentication (can be disabled later for key-only)
PasswordAuthentication yes

# Optional: Limit login attempts (prevents brute force)
MaxAuthTries 3
LoginGraceTime 60

# Optional: Disable empty passwords
PermitEmptyPasswords no
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

---

## Step 3: Enable SSH Service

```bash
# Enable SSH service to start on boot
systemctl enable sshd

# Start SSH service immediately (if after first boot)
systemctl start sshd
```

**Note:** If you're in chroot, service will start automatically on first boot.

---

## Step 4: Verify Configuration

```bash
# Check SSH service status
systemctl status sshd

# Should show: active (running)

# Verify SSH is listening on port 1991
ss -tlnp | grep :1991

# Should show: LISTEN on port 1991
```

---

## Step 5: Test SSH Connection (After First Boot)

**From another computer:**

```bash
# Connect to SSH server
ssh -p 1991 <USERNAME>@<SERVER_IP>

# Replace:
# <USERNAME> - Your user account (not root!)
# <SERVER_IP> - IP address of your Arch Linux system
```

**Expected:**
- Connection prompt appears
- Password prompt (enter user password, not root)
- Successful login

---

## Security Notes

- **Port 1991:** Custom port reduces automated attacks (most bots scan port 22)
- **Root login disabled:** Prevents direct root access (use sudo instead)
- **Password authentication:** Enabled by default (can disable for key-only later)
- **Firewall:** Configure UFW firewall (see `22-ufw-firewall.md`) to allow port 1991
- **Fail2ban:** Install fail2ban (see `23-fail2ban.md`) to prevent brute force attacks

---

## Verification

Run these commands to verify SSH configuration:

```bash
# Check SSH service is running
systemctl is-active sshd
# Expected: active

# Check SSH is listening on correct port
ss -tlnp | grep :1991
# Expected: LISTEN 0 128 0.0.0.0:1991

# Verify root login is disabled
grep "PermitRootLogin" /etc/ssh/sshd_config
# Expected: PermitRootLogin no

# Verify port is set correctly
grep "^Port" /etc/ssh/sshd_config
# Expected: Port 1991
```

---

## Troubleshooting

### Problem: SSH service fails to start
**Solution:**
1. Check configuration syntax: `sshd -t`
2. Check logs: `journalctl -u sshd`
3. Verify port 1991 is not in use: `ss -tlnp | grep :1991`

### Problem: Cannot connect from remote computer
**Solution:**
1. Check firewall allows port 1991: `ufw status` (if UFW is installed)
2. Verify SSH service is running: `systemctl status sshd`
3. Check network connectivity: `ping <SERVER_IP>`
4. Verify correct port: `ssh -p 1991 <USERNAME>@<SERVER_IP>`

### Problem: "Permission denied" when connecting
**Solution:**
1. Verify you're using correct username (not root)
2. Check user password is correct
3. Verify user account exists: `id <USERNAME>`
4. Check SSH logs: `journalctl -u sshd -n 50`

---

**SUCCESS:** SSH server configured and running on port 1991

**Official Resources:**
- [ArchWiki: OpenSSH](https://wiki.archlinux.org/title/OpenSSH)
- [SSH Configuration Manual](https://man.archlinux.org/man/sshd_config.5)

**Next:**
- `22-ufw-firewall.md` - Configure firewall to allow SSH
- `23-fail2ban.md` - Install fail2ban to prevent brute force attacks

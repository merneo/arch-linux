# Module: SSH Server Configuration

**Purpose:** Install and configure SSH (Secure Shell) server with custom port and security settings. SSH provides a secure channel over an unsecured network by using strong cryptography, making it essential for remote administration. For comprehensive information, refer to the [ArchWiki on OpenSSH](https://wiki.archlinux.org/title/OpenSSH).

**Prerequisites:**
- Inside chroot environment (module `chroot.md`) OR after first boot
- Network connection established

**Time:** 5-10 minutes

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot

**Note:** This module configures SSH server with:
- Custom port: **1991**
- Root login: **DISABLED** (security best practice)
- Password authentication: **ENABLED** (can be disabled later for key-only)

---

## Step 1: Install SSH Server

The `openssh` package provides the SSH client and server utilities. The SSH server daemon is `sshd`. For more details, refer to the [ArchWiki on OpenSSH](https://wiki.archlinux.org/title/OpenSSH#Installation).

```bash
pacman -S openssh
```

---

## Step 2: Configure SSH Server

The main configuration file for the SSH server is `/etc/ssh/sshd_config`. It is critical to configure this file carefully to ensure security. For a detailed explanation of all available options, consult `man sshd_config` or the [ArchWiki on OpenSSH/Configuration](https://wiki.archlinux.org/title/OpenSSH#Configuration).

```bash
# Edit SSH server configuration
nano /etc/ssh/sshd_config
```

**Find and modify these lines:**

```bash
# Change SSH port from 22 to 1991. Changing the default port (22) is a common security practice
# to reduce automated scanning attempts.
Port 1991

# Disable root login (security best practice). Direct root login via SSH is generally
# discouraged as it can lead to easier brute-force attacks and bypass audit trails.
# Instead, users should log in with a regular account and use 'sudo'.
PermitRootLogin no

# Allow password authentication. This can be disabled later for key-only authentication
# (a more secure method) once SSH keys are set up.
PasswordAuthentication yes

# Optional: Limit login attempts (prevents brute force). 'MaxAuthTries' defines the maximum
# number of authentication attempts permitted per connection. 'LoginGraceTime' is the time
# limit for authentication.
MaxAuthTries 3
LoginGraceTime 60

# Optional: Disable empty passwords. Ensures that users with empty passwords cannot log in.
PermitEmptyPasswords no
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

---

## Step 3: Enable SSH Service

Services in Arch Linux are managed by `systemd`. The `systemctl enable sshd` command ensures the SSH daemon (`sshd`) starts automatically at boot. For more details on `systemd` and service management, refer to the [ArchWiki on systemd](https://wiki.archlinux.org/title/Systemd).

```bash
# Enable SSH service to start on boot
systemctl enable sshd

# If after first boot (not in chroot), start SSH service immediately
systemctl start sshd
```

**Note:** 
- **In chroot:** Use only `systemctl enable sshd` (service will start automatically on first boot)
- **After first boot:** Use `systemctl enable --now sshd` to enable and start immediately

---

## Step 4: Verify Configuration

Verifying the SSH service status and ensuring it's listening on the configured port is crucial.

```bash
# Check SSH service status. For systemctl status details, see [ArchWiki: systemd](https://wiki.archlinux.org/title/Systemd#Using_units).
systemctl status sshd

# Should show: active (running)

# Verify SSH is listening on port 1991. The 'ss' (socket statistics) command is used to show active socket connections. For more details, consult 'man ss'.
ss -tlnp | grep :1991

# Should show: LISTEN on port 1991
```

---

## Step 5: Test SSH Connection (After First Boot)

Testing the SSH connection from another computer verifies that the server is accessible and correctly configured. The `ssh` command is the client-side utility for connecting to an SSH server. For more details on using the SSH client, refer to the [ArchWiki on OpenSSH#Usage](https://wiki.archlinux.org/title/OpenSSH#Usage).

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
- **Firewall:** Configure UFW firewall (see `ufw-firewall.md`) to allow port 1991
- **Fail2ban:** Install fail2ban (see `fail2ban.md`) to prevent brute force attacks

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

For more extensive troubleshooting on OpenSSH issues, refer to the [ArchWiki on OpenSSH/Troubleshooting](https://wiki.archlinux.org/title/OpenSSH/Troubleshooting).

### Problem: SSH service fails to start
**Solution:**
1. Check configuration syntax: `sshd -t`. This command checks the validity of the SSH daemon configuration file without attempting to start the service.
2. Check logs: `journalctl -u sshd`. For `journalctl` details, see [ArchWiki: systemd/Journal](https://wiki.archlinux.org/title/Systemd/Journal).
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
- `ufw-firewall.md` - Configure firewall to allow SSH
- `fail2ban.md` - Install fail2ban to prevent brute force attacks

# Module: Fail2ban Configuration for SSH

**Purpose:** Install and configure fail2ban to prevent SSH brute force attacks. [Fail2ban](https://wiki.archlinux.org/title/Fail2ban) is an intrusion prevention framework that scans log files (e.g., `/var/log/auth.log` or `journalctl` output) for malicious activity and automatically bans IP addresses that show signs of repeated authentication failures.

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`) OR after first boot
- SSH server configured (module `21-ssh-server.md`)
- UFW firewall configured (module `22-ufw-firewall.md`) - recommended

**Time:** 5-10 minutes

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot

**Note:** This module configures fail2ban to:
- Monitor SSH login attempts on port 1991
- Ban IP addresses after failed login attempts
- Protect against brute force attacks

---

## Step 1: Install Fail2ban

The `fail2ban` package contains the daemon and configuration files for fail2ban. It is available in the official Arch Linux repositories.

```bash
pacman -S fail2ban
```

---

## Step 2: Create Fail2ban Configuration

Arch Linux uses the `systemd journal` for logging by default, so fail2ban is configured to use the `systemd` backend for monitoring logs. This configuration defines a "jail" for SSH to specify monitoring parameters.

```bash
# Create local configuration directory
mkdir -p /etc/fail2ban/jail.d

# Create SSH jail configuration for port 1991 with systemd backend
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
```

**Configuration explanation:** For a full understanding of these parameters, refer to the [Fail2ban documentation](https://www.fail2ban.org/wiki/index.php/MANUAL_0_8#Jails).
- `enabled = true`: Activates the SSH jail.
- `port = 1991`: Specifies the port to monitor for failed SSH attempts (matches our custom SSH port).
- `filter = sshd`: Uses the default SSH filter defined by Fail2ban to identify login failures.
- `backend = systemd`: Configures Fail2ban to use the `systemd journal` for log monitoring. See [ArchWiki: systemd/Journal](https://wiki.archlinux.org/title/Systemd/Journal).
- `journalmatch = _SYSTEMD_UNIT=sshd.service`: Specifies to monitor logs specifically from the `sshd.service` unit.
- `maxretry = 3`: The number of failed attempts before an IP address is banned.
- `bantime = 3600`: The duration (in seconds) for which an IP address is banned (here, 1 hour).
- `findtime = 600`: The duration (in seconds) within which `maxretry` failures must occur to trigger a ban (here, 10 minutes).

---

## Step 3: Enable and Start Fail2ban

Like other services in Arch Linux, fail2ban is managed by `systemd`. Enabling the service ensures it starts automatically at boot, and starting it immediately activates the protection. For details on `systemd` service management, refer to the [ArchWiki on systemd](https://wiki.archlinux.org/title/Systemd).

```bash
# Enable fail2ban service
systemctl enable fail2ban

# Start fail2ban service
systemctl start fail2ban
```

**Note:** If you're in chroot, service will start automatically on first boot.

---

## Step 4: Verify Fail2ban Configuration

Verifying the status of fail2ban and its configured jails is important to ensure it's actively protecting your services. The `fail2ban-client` utility provides commands to interact with the fail2ban server. For more details, consult `man fail2ban-client`.

```bash
# Check fail2ban service status
systemctl status fail2ban

# Should show: active (running)

# Check fail2ban jails
fail2ban-client status

# Should show:
# Status
# |- Number of jail: 1
# `- Jail list: sshd

# Check SSH jail status
fail2ban-client status sshd

# Should show:
# Status for the jail: sshd
# |- Filter
# |  |- Currently failed: 0
# |  |- Total failed:     0
# |  `- File list:        /var/log/auth.log
# `- Actions
#    |- Currently banned: 0
#    |- Total banned:     0
#    `- Banned IP list:
```

---

## Step 5: Test Fail2ban (After First Boot)

Testing Fail2ban's effectiveness involves intentionally triggering its ban mechanism by attempting multiple failed SSH logins. The `ssh` client will be used for these attempts. For more on SSH client usage, refer to the [ArchWiki on OpenSSH#Usage](https://wiki.archlinux.org/title/OpenSSH#Usage).

**From another computer (or terminal):**

```bash
# Try to connect with wrong password 3 times
ssh -p 1991 wronguser@<SERVER_IP>
# Enter wrong password 3 times

# Check if IP is banned
fail2ban-client status sshd

# Should show banned IP in "Banned IP list"
```

---

## Security Notes

- **Max retry = 3:** IP is banned after 3 failed login attempts
- **Ban time = 1 hour:** Banned IPs are blocked for 1 hour
- **Find time = 10 minutes:** Failures are counted within 10-minute window
- **Works with UFW:** Fail2ban automatically updates UFW rules to ban IPs
- **Monitoring:** Fail2ban monitors SSH logs for failed login attempts

---

## Verification

Run these commands to verify fail2ban:

```bash
# Check fail2ban service
systemctl is-active fail2ban
# Expected: active

# Check SSH jail is enabled
fail2ban-client status sshd
# Expected: Status for the jail: sshd

# Check banned IPs (should be empty initially)
fail2ban-client status sshd | grep "Banned IP"
# Expected: Banned IP list: (empty)
```

---

## Troubleshooting

For more extensive troubleshooting on Fail2ban related issues, refer to the [ArchWiki on Fail2ban#Troubleshooting](https://wiki.archlinux.org/title/Fail2ban#Troubleshooting).

### Problem: Fail2ban not detecting failed logins
**Solution:**
1. Check backend: `fail2ban-client status sshd | grep -i backend`
2. Verify SSH logs are being written: `journalctl -u sshd`. For `journalctl` details, see [ArchWiki: systemd/Journal](https://wiki.archlinux.org/title/Systemd/Journal).
3. Test filter with systemd: `fail2ban-regex "journalctl _SYSTEMD_UNIT=sshd.service" /etc/fail2ban/filter.d/sshd.conf`

### Problem: Fail2ban not banning IPs
**Solution:**
1. Check fail2ban is running: `systemctl status fail2ban`
2. Verify jail is enabled: `fail2ban-client status sshd`
3. Check UFW integration: `ufw status | grep fail2ban`

### Problem: Cannot unban IP
**Solution:**
```bash
# Unban specific IP
fail2ban-client set sshd unbanip <IP_ADDRESS>

# Unban all IPs
fail2ban-client set sshd unbanip --all
```

---

## Advanced Configuration

### Adjust Ban Time and Retry Count

Edit `/etc/fail2ban/jail.d/sshd.conf`:

```bash
nano /etc/fail2ban/jail.d/sshd.conf
```

**Modify values:**
```bash
maxretry = 5        # Ban after 5 attempts (instead of 3)
bantime = 7200      # Ban for 2 hours (instead of 1)
findtime = 300      # Count failures within 5 minutes (instead of 10)
```

**Reload fail2ban:**
```bash
systemctl reload fail2ban
```

---

**SUCCESS:** Fail2ban configured and protecting SSH

**Official Resources:**
- [ArchWiki: Fail2ban](https://wiki.archlinux.org/title/Fail2ban)
- [Fail2ban Documentation](https://www.fail2ban.org/wiki/index.php/Main_Page)

**Next:** Continue with other configuration modules

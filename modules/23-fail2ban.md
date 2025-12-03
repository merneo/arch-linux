# Module: Fail2ban Configuration for SSH

**Purpose:** Install and configure fail2ban to prevent SSH brute force attacks

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

```bash
pacman -S fail2ban
```

---

## Step 2: Create Fail2ban Configuration

**Arch Linux uses systemd journal by default, so we configure fail2ban to use systemd backend:**

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

**Configuration explanation:**
- `enabled = true`: Enable SSH jail
- `port = 1991`: Monitor SSH on custom port
- `filter = sshd`: Use default SSH filter
- `backend = systemd`: Use systemd journal (Arch Linux default)
- `journalmatch = _SYSTEMD_UNIT=sshd.service`: Monitor SSH service logs
- `maxretry = 3`: Ban after 3 failed attempts
- `bantime = 3600`: Ban for 1 hour (3600 seconds)
- `findtime = 600`: Count failures within 10 minutes (600 seconds)

---

## Step 3: Enable and Start Fail2ban

```bash
# Enable fail2ban service
systemctl enable fail2ban

# Start fail2ban service
systemctl start fail2ban
```

**Note:** If you're in chroot, service will start automatically on first boot.

---

## Step 4: Verify Fail2ban Configuration

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

### Problem: Fail2ban not detecting failed logins
**Solution:**
1. Check backend: `fail2ban-client status sshd | grep -i backend`
2. Verify SSH logs are being written: `journalctl -u sshd -n 50`
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

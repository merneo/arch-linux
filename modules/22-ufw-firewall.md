# Module: UFW Firewall Configuration

**Purpose:** Install and configure UFW (Uncomplicated Firewall) with SSH access

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`) OR after first boot
- SSH server configured (module `21-ssh-server.md`) - if using SSH

**Time:** 5-10 minutes

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot

**Note:** This module configures UFW firewall with:
- **All outgoing traffic:** ALLOWED
- **Incoming SSH (port 1991):** ALLOWED
- **All other incoming traffic:** DENIED (default)

---

## Step 1: Install UFW

```bash
pacman -S ufw
```

---

## Step 2: Configure UFW Default Policies

```bash
# Set default policies:
# - Deny all incoming traffic (default)
# - Allow all outgoing traffic (default)
ufw default deny incoming
ufw default allow outgoing
```

**Explanation:**
- `deny incoming`: Blocks all incoming connections by default
- `allow outgoing`: Allows all outgoing connections (web browsing, updates, etc.)

---

## Step 3: Allow SSH on Port 1991

```bash
# Allow incoming SSH connections on port 1991
ufw allow 1991/tcp

# Optional: Add comment for clarity
ufw allow 1991/tcp comment 'SSH server'
```

**Verify rule:**
```bash
ufw status numbered

# Should show:
# [1] 1991/tcp                     ALLOW IN    Anywhere
# [2] 1991/tcp (v6)                ALLOW IN    Anywhere (v6)
```

---

## Step 4: Enable UFW Firewall

```bash
# Enable UFW firewall
ufw enable

# Confirm when prompted:
# Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
```

**Warning:** If you're connected via SSH, enabling UFW will not disconnect you (SSH is already allowed).

---

## Step 5: Verify UFW Status

```bash
# Check UFW status
ufw status verbose

# Expected output:
# Status: active
# Logging: on (low)
# Default: deny (incoming), allow (outgoing), disabled (routed)
# New profiles: skip
#
# To                         Action      From
# --                         ------      ----
# 1991/tcp                   ALLOW IN    Anywhere
# 1991/tcp (v6)              ALLOW IN    Anywhere (v6)
```

---

## Step 6: Test Firewall (After First Boot)

**From another computer:**

```bash
# Test SSH connection (should work)
ssh -p 1991 <USERNAME>@<SERVER_IP>

# Test blocked port (should fail)
telnet <SERVER_IP> 80
# Expected: Connection refused or timeout
```

---

## Additional UFW Rules (Optional)

### Allow HTTP/HTTPS (if running web server):

```bash
ufw allow 80/tcp comment 'HTTP'
ufw allow 443/tcp comment 'HTTPS'
```

### Allow specific IP address:

```bash
# Allow SSH only from specific IP
ufw allow from <TRUSTED_IP> to any port 1991 proto tcp
```

### Allow local network:

```bash
# Allow SSH from local network (192.168.1.0/24)
ufw allow from 192.168.1.0/24 to any port 1991 proto tcp
```

---

## Security Notes

- **Default deny incoming:** All incoming traffic is blocked except explicitly allowed ports
- **SSH port 1991:** Only SSH is allowed from outside
- **Outgoing allowed:** System can access internet, download updates, etc.
- **Fail2ban recommended:** Install fail2ban (see `23-fail2ban.md`) to prevent SSH brute force attacks

---

## Verification

Run these commands to verify UFW configuration:

```bash
# Check UFW is active
ufw status
# Expected: Status: active

# Check SSH port is allowed
ufw status | grep 1991
# Expected: 1991/tcp ALLOW IN

# Check default policies
ufw status verbose | grep Default
# Expected: Default: deny (incoming), allow (outgoing)
```

---

## Troubleshooting

### Problem: UFW blocks SSH connection
**Solution:**
1. Check SSH port is allowed: `ufw status | grep 1991`
2. If missing, add rule: `ufw allow 1991/tcp`
3. Reload UFW: `ufw reload`

### Problem: Cannot access internet after enabling UFW
**Solution:**
1. Check outgoing is allowed: `ufw status verbose | grep "Default.*outgoing"`
2. Should show: `allow (outgoing)`
3. If not, set: `ufw default allow outgoing`

### Problem: UFW not active
**Solution:**
1. Check UFW status: `ufw status`
2. If inactive, enable UFW: `ufw enable`
3. Verify rules: `ufw status verbose`
4. **Note:** UFW doesn't use systemd service in Arch Linux - it's managed directly via `ufw` commands

---

**SUCCESS:** UFW firewall configured and active

**Official Resources:**
- [ArchWiki: UFW](https://wiki.archlinux.org/title/Uncomplicated_Firewall)
- [UFW Documentation](https://help.ubuntu.com/community/UFW)

**Next:**
- `23-fail2ban.md` - Install fail2ban to prevent SSH brute force attacks

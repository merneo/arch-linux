# Phase 07: Security Configuration

**Purpose:** Configure SSH, firewall, and intrusion prevention

**Prerequisites:**
- Inside chroot environment OR after first boot
- Network configured

**Steps (execute in order):**

1. **[SSH Server](../steps/21-ssh-server.md)**
   - Install and configure SSH (port 1991, root disabled)

2. **[UFW Firewall](../steps/22-ufw-firewall.md)**
   - Install and configure UFW
   - Allow outgoing, only SSH incoming

3. **[Fail2ban](../steps/23-fail2ban.md)**
   - Install and configure fail2ban
   - Protect SSH from brute force attacks

## Verification

```bash
systemctl is-active sshd
ss -tlnp | grep :1991
ufw status
fail2ban-client status sshd
```

## Next Phase

**[Phase 08: Laptop Hardware](08-LAPTOP_HARDWARE.md)** (laptops only) or **[Phase 09: Finalize](09-FINALIZE.md)**

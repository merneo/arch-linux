# Phase 07: Security Configuration

**Purpose:** Configure SSH, firewall, and intrusion prevention. This phase focuses on hardening your system against unauthorized access and protecting network services. For a comprehensive overview of security best practices in Arch Linux, refer to the [ArchWiki on Security](https://wiki.archlinux.org/title/Security).

**Prerequisites:**
- Inside chroot environment OR after first boot
- Network configured

**Time Estimate:** 15-20 minutes
**Difficulty:** ⭐⭐ Intermediate

**Steps (execute in order):**

1. **[SSH Server](../steps/ssh-server-commands.md)**
   - Install and configure SSH (port 1991, root disabled)

2. **[UFW Firewall](../steps/ufw-firewall-commands.md)**
   - Install and configure UFW
   - Allow outgoing, only SSH incoming

3. **[Fail2ban](../steps/fail2ban-commands.md)**
   - Install and configure fail2ban
   - Protect SSH from brute force attacks

## Verification

```bash
systemctl is-active sshd
ss -tlnp | grep :1991
ufw status
fail2ban-client status sshd
```

---

## Navigation

**Previous:** 
- **[← Phase 06: Audio](06-audio.md)** (if you did audio)
- **[← Phase 05: Network](05-network.md)** (if you skipped audio)

**Next:**
- **[Phase 08: Hardware →](08-hardware.md)** (laptops only - if you have laptop hardware)
- **[Phase 09: Finalize →](09-finalize.md)** (desktops or if skipping hardware)

---

**Back to:** [Main Index](../index.md) | [Repository Root](../README.md)

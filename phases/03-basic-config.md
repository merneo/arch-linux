# Phase 03: Basic Configuration

**Purpose:** Configure locale, timezone, and user account. This phase involves setting up fundamental system parameters that define your system's language, time, and user access privileges. For general configuration guidance, refer to the [ArchWiki Installation Guide#Configuration](https://wiki.archlinux.org/title/Installation_guide#Configuration).

**Prerequisites:**
- Inside chroot environment

**Time Estimate:** 10-15 minutes
**Difficulty:** ⭐ Easy

**Steps (execute in order):**

1. **[Locale and Timezone](../steps/locale-commands.md)**
   - Set system locale
   - Set timezone

2. **[User Creation](../steps/user-creation-commands.md)**
   - Create user account
   - Configure sudo access

## Verification

```bash
locale
timedatectl status
id <USERNAME>
```

---

## Navigation

**Previous:** **[← Phase 02: System Install](02-system-install.md)**

**Next:** **[Phase 04: Bootloader →](04-bootloader.md)**

---

**Back to:** [Main Index](../index.md) | [Repository Root](../README.md)

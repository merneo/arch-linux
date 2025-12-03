# Phase 03: Basic Configuration

**Purpose:** Configure locale, timezone, root password, and user account

**Prerequisites:**
- Inside chroot environment

**Steps (execute in order):**

1. **[Locale and Timezone](../steps/02-locale.md)**
   - Set system locale
   - Set timezone

2. **[User Creation](../steps/04-user-creation.md)**
   - Create user account
   - Configure sudo access

## Verification

```bash
locale
timedatectl status
id <USERNAME>
```

## Next Phase

**[Phase 04: Bootloader](04-BOOTLOADER.md)**

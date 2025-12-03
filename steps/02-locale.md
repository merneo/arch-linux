# Step: Locale and Timezone Configuration

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment

## Commands

```bash
# Set timezone (replace Europe/Prague with your timezone)
ln -sf /usr/share/zoneinfo/Europe/Prague /etc/localtime
hwclock --systohc

# Uncomment locale in locale.gen (replace en_US.UTF-8 with your locale)
sed -i 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
locale-gen

# Set system locale
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

## Verification

```bash
locale
timedatectl status
```

**NEXT:** `03-root-password.md`

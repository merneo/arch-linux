# Step: Locale and Timezone Configuration

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment

## Commands

```bash
# Set timezone (replace Europe/Prague with your timezone). For more details, see [ArchWiki: Time#Time_zone](https://wiki.archlinux.org/title/Time#Time_zone) and [ArchWiki: System time](https://wiki.archlinux.org/title/System_time).
ln -sf /usr/share/zoneinfo/Europe/Prague /etc/localtime
hwclock --systohc

# Uncomment locale in locale.gen (replace en_US.UTF-8 with your locale). Refer to [ArchWiki: Locale#Generate locales](https://wiki.archlinux.org/title/Locale#Generate_locales).
sed -i 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
locale-gen

# Set system locale. See [ArchWiki: Locale#Set the system locale](https://wiki.archlinux.org/title/Locale#Set_the_system_locale).
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

## Verification

```bash
locale # See [ArchWiki: Locale#Verification](https://wiki.archlinux.org/title/Locale#Verification).
timedatectl status # See [ArchWiki: Time#Time_zone_settings](https://wiki.archlinux.org/title/Time#Time_zone_settings).
```

**NEXT:** `03-root-password.md`

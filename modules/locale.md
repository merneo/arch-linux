# Module: Locale and Timezone Configuration

**Purpose:** Configure system locale and timezone. This is crucial for proper display of language-specific characters, date/time formats, and system time synchronization. For detailed information, refer to the [ArchWiki on Locale](https://wiki.archlinux.org/title/Locale) and [ArchWiki on Time](https://wiki.archlinux.org/title/Time).

**Prerequisites:**
- Inside chroot environment (module `chroot.md`)

**Time:** 2-3 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## Placeholders Used in This Module

- `<LOCALE>`: Your locale (e.g., `en_US.UTF-8`, `cs_CZ.UTF-8`, `de_DE.UTF-8`)
- `<TIMEZONE>`: Your timezone (e.g., `Europe/Prague`, `America/New_York`, `Asia/Tokyo`)

---

## Step 1: Set Timezone

Setting the correct timezone is vital for accurate system logging, scheduling, and overall time management. For detailed guidance on time zones, refer to the [ArchWiki on Time zones](https://wiki.archlinux.org/title/Time#Time_zone). The `hwclock` command is used for managing the hardware clock. See [ArchWiki on System time](https://wiki.archlinux.org/title/System_time).

```bash
# Replace with your timezone (examples: Europe/Prague, America/New_York, Asia/Tokyo)
ln -sf /usr/share/zoneinfo/Europe/Prague /etc/localtime

# Generate /etc/adjtime
hwclock --systohc
```

**Verify timezone:**
```bash
timedatectl status
```

---

## Step 2: Set Locale

The system locale defines language, collation, character type, and formatting conventions. Enabling the correct locale is crucial for proper text display. For more information, refer to the [ArchWiki on Locale#Generate locales](https://wiki.archlinux.org/title/Locale#Generate_locales).

```bash
# Uncomment your locale in locale.gen
# For English (US):
sed -i 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen

# For other locales, edit manually:
# nano /etc/locale.gen
# Uncomment desired locale (e.g., cs_CZ.UTF-8, de_DE.UTF-8)

# Generate locales
locale-gen
```

---

## Step 3: Set System Locale

This step sets the `LANG` environment variable, which defines the default locale for the entire system. For detailed instructions on configuring the system locale, refer to the [ArchWiki on Locale#Set the system locale](https://wiki.archlinux.org/title/Locale#Set_the_system_locale).

```bash
# Set default locale
echo "LANG=en_US.UTF-8" > /etc/locale.conf

# Or for other locales:
# echo "LANG=cs_CZ.UTF-8" > /etc/locale.conf
```

**Verify locale:**
```bash
cat /etc/locale.conf
```

---

**SUCCESS:** Locale and timezone configured

## Verification

Run these commands to verify configuration. For more details on locale verification, see [ArchWiki: Locale#Verification](https://wiki.archlinux.org/title/Locale#Verification). For time zone settings, refer to [ArchWiki: Time#Time_zone_settings](https://wiki.archlinux.org/title/Time#Time_zone_settings).

```bash
# Check locale
locale

# Should show your configured locale (e.g., LANG=en_US.UTF-8)

# Check timezone
timedatectl status

# Should show your configured timezone (e.g., Time zone: Europe/Prague (CET, +0100))
```

**Next:** Continue with other configuration modules

---

## Troubleshooting

For more general troubleshooting of locale and timezone issues, refer to the respective [ArchWiki: Locale](https://wiki.archlinux.org/title/Locale#Troubleshooting) and [ArchWiki: Time](https://wiki.archlinux.org/title/Time#Troubleshooting) sections.

### Problem: locale-gen fails
**Solution:**
1. Verify locale is uncommented in `/etc/locale.gen`
2. Check file syntax: `cat /etc/locale.gen | grep -v "^#" | grep -v "^$"`
3. Regenerate: `locale-gen`

### Problem: Wrong timezone after reboot
**Solution:**
1. Verify symlink: `ls -la /etc/localtime`
2. Should point to correct timezone: `/etc/localtime -> /usr/share/zoneinfo/<TIMEZONE>`
3. Recreate if wrong: `ln -sf /usr/share/zoneinfo/<TIMEZONE> /etc/localtime`

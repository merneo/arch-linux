# Module: Locale and Timezone Configuration

**Purpose:** Configure system locale and timezone

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)

**Time:** 2-3 minutes

---

## Step 1: Set Timezone

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

**Next:** Continue with other configuration modules

# Příklady Transformace: Moduly → Kroky → Fáze

## Příklad 1: Transformace `02-locale.md`

### PŘED (aktuální modul - 106 řádků)

```markdown
# Module: Locale and Timezone Configuration

**Purpose:** Configure system locale and timezone
**Prerequisites:** Inside chroot environment
**Time:** 2-3 minutes
**ENVIRONMENT:** Chroot (root@archiso /)#

## Placeholders Used in This Module
- `<LOCALE>`: Your locale
- `<TIMEZONE>`: Your timezone

## Step 1: Set Timezone
[dlouhé vysvětlení]
[komentáře]
[příkazy]

## Step 2: Set Locale
[dlouhé vysvětlení]
[komentáře]
[příkazy]

## Step 3: Set System Locale
[dlouhé vysvětlení]
[komentáře]
[příkazy]

## Verification
[verifikační příkazy]

## Troubleshooting
[řešení problémů]
```

### PO (transformovaný krok - ~30 řádků)

```markdown
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
```

**Úspora:** 106 řádků → ~30 řádků (72% redukce)

---

## Příklad 2: Transformace `00-core-installation.md`

### PŘED (aktuální modul - 137 řádků)

Obsahuje:
- Prerequisites checklist
- Dlouhé vysvětlení každého kroku
- Troubleshooting sekci
- Verification sekci

### PO (transformovaný krok - ~25 řádků)

```markdown
# Step: Core System Installation

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Network connected, partitions mounted at /mnt

## Commands

```bash
# Update mirrorlist (optional, faster mirrors)
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup
reflector --country US,Germany,France --age 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist

# Install base system
pacstrap /mnt base base-devel linux linux-firmware linux-headers btrfs-progs dosfstools e2fsprogs networkmanager network-manager-applet vim nano git sudo man-db man-pages

# Generate fstab
genfstab -U /mnt >> /mnt/etc/fstab
```

## Verification

```bash
ls -la /mnt/bin /mnt/etc /mnt/usr
cat /mnt/etc/fstab
```

**NEXT:** `01-chroot.md`
```

**Úspora:** 137 řádků → ~25 řádků (82% redukce)

---

## Příklad 3: Fáze - `01-DISK_SETUP.md`

```markdown
# Phase 01: Disk Setup

**Purpose:** Configure disk partitions, encryption, and filesystem

**Prerequisites:**
- Booted from Arch Linux Live USB
- Disk identified

**Steps (execute in order):**

1. **[Disk Partitioning](../steps/06-disk-partitioning.md)**
   - Create EFI, root, and swap partitions
   - Choose: Single boot or Dual boot layout

2. **[LUKS Encryption](../steps/07-luks-encryption.md)** (if using encryption)
   - Encrypt root and swap partitions
   - Set strong passphrase

3. **[Btrfs Filesystem](../steps/08-btrfs-filesystem.md)**
   - Create Btrfs on encrypted or unencrypted partition
   - Create subvolumes: @, @home, @log, @cache, @snapshots

4. **[Mount Partitions](../steps/09-mount-partitions.md)** (if not using Btrfs)
   - Mount partitions for installation

## Verification

```bash
# Check partitions
lsblk -f

# Check mounts
mount | grep /mnt
```

## Next Phase

**[Phase 02: System Install](02-SYSTEM_INSTALL.md)**
```

---

## Příklad 4: Kompletní Fáze - `02-SYSTEM_INSTALL.md`

```markdown
# Phase 02: System Install

**Purpose:** Install base Arch Linux system and enter chroot

**Prerequisites:**
- Disk partitions created and mounted at /mnt
- Network connection established

**Steps (execute in order):**

1. **[Core System Installation](../steps/00-core-installation.md)**
   - Install base system with pacstrap
   - Generate fstab

2. **[Enter Chroot](../steps/01-chroot.md)**
   - Enter chroot environment for configuration

## Verification

```bash
# Check system installed
ls -la /mnt/bin /mnt/etc /mnt/usr

# Verify in chroot
arch-chroot /mnt
```

## Next Phase

**[Phase 03: Basic Config](03-BASIC_CONFIG.md)**
```

---

## Příklad 5: Transformovaný Krok - `07-luks-encryption.md`

### PŘED (188 řádků)
- Dlouhé vysvětlení
- Security notes
- Placeholders
- Troubleshooting

### PO (~40 řádků)

```markdown
# Step: LUKS2 Encryption Setup

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Disk partitioned, root partition identified

## Commands

```bash
# Encrypt root partition (replace /dev/sdX2 with your root partition)
cryptsetup luksFormat --type luks2 \
  --cipher aes-xts-plain64 \
  --key-size 512 \
  --pbkdf argon2id \
  --iter-time 5000 \
  /dev/sdX2
# Type: YES (uppercase)
# Enter strong passphrase (min 20 chars)

# Open encrypted root partition
cryptsetup open /dev/sdX2 cryptroot

# Encrypt swap partition (if exists, replace /dev/sdX3)
cryptsetup luksFormat --type luks2 /dev/sdX3
# Type: YES
# Enter passphrase (can be same as root)

# Open encrypted swap
cryptsetup open /dev/sdX3 cryptswap
```

## Verification

```bash
# Check encrypted partitions
lsblk
# Should show: cryptroot, cryptswap

# Check LUKS info
cryptsetup luksDump /dev/sdX2
```

**NEXT:** `08-btrfs-filesystem.md`
```

**Úspora:** 188 řádků → ~40 řádků (79% redukce)

---

## Struktura Složek - Finální Návrh

```
arch-linux/
├── phases/                          # Fáze (soubory kroků)
│   ├── 00-PREPARATION.md
│   ├── 01-DISK_SETUP.md
│   ├── 02-SYSTEM_INSTALL.md
│   ├── 03-BASIC_CONFIG.md
│   ├── 04-BOOTLOADER.md
│   ├── 05-NETWORK.md
│   ├── 06-AUDIO.md
│   ├── 07-SECURITY.md
│   ├── 08-LAPTOP_HARDWARE.md
│   └── 09-FINALIZE.md
│
├── steps/                            # Kroky (příkazy)
│   ├── PRE-01-create-arch-usb.md
│   ├── PRE-02-install-windows.md
│   ├── PRE-03-format-disk.md
│   ├── 00-core-installation.md
│   ├── 01-chroot.md
│   ├── 02-locale.md
│   ├── 03-root-password.md
│   ├── 04-user-creation.md
│   ├── 05-grub.md
│   ├── 06-disk-partitioning.md
│   ├── 07-luks-encryption.md
│   ├── 08-btrfs-filesystem.md
│   ├── 09-mount-partitions.md
│   ├── 10-luks-keyfile-auto-unlock.md
│   ├── 11-networkmanager.md
│   ├── 12-wifi.md
│   ├── 13-bluetooth.md
│   ├── 14-audio.md
│   ├── 15-exit-chroot.md
│   ├── 17-laptop-touchpad.md
│   ├── 18-laptop-webcam.md
│   ├── 19-laptop-ir-camera.md
│   ├── 20-laptop-fingerprint.md
│   ├── 21-ssh-server.md
│   ├── 22-ufw-firewall.md
│   └── 23-fail2ban.md
│
├── docs/                             # Dokumentace (zůstává)
│   ├── INSTALLATION-SCENARIOS.md
│   ├── STEP_INDEX.md (přejmenováno z MODULE_INDEX.md)
│   ├── TROUBLESHOOTING.md (nový, obsahuje troubleshooting ze všech kroků)
│   └── REFERENCE.md (nový, obsahuje dlouhé vysvětlení)
│
└── wiki/                             # Wiki (zůstává)
    └── ...
```

---

## Výhody Transformace

### 1. Redukce Velikosti
- **Aktuálně:** ~3812 řádků v modulech
- **Po transformaci:** ~1000 řádků v krocích (74% redukce)
- **Fáze:** ~500 řádků (přehledy)
- **Celkem:** ~1500 řádků vs 3812 (61% redukce)

### 2. Čistá Struktura
- **Fáze** = logické skupiny
- **Kroky** = jen příkazy
- **Příkazy** = konkrétní shell příkazy

### 3. Snadná Navigace
- Fáze → kroky → příkazy
- Jasná hierarchie
- Snadné generování procedur

### 4. Automatizace
- Snadné parsování kroků
- Generování skriptů z kroků
- Validace příkazů

---

## Implementační Plán

### Fáze 1: Proof of Concept (2-3 moduly)
1. Transformovat `02-locale.md` → `steps/02-locale.md`
2. Transformovat `00-core-installation.md` → `steps/00-core-installation.md`
3. Vytvořit `phases/02-SYSTEM_INSTALL.md`
4. Otestovat strukturu

### Fáze 2: Transformace Všech Modulů
1. Transformovat všechny moduly na kroky
2. Přesunout do `steps/`
3. Odstranit dlouhé texty (přesunout do `docs/`)

### Fáze 3: Vytvoření Fází
1. Vytvořit všechny fáze
2. Propojit s kroky
3. Aktualizovat odkazy

### Fáze 4: Aktualizace Dokumentace
1. Aktualizovat `INSTALLATION-SCENARIOS.md`
2. Přejmenovat `MODULE_INDEX.md` → `STEP_INDEX.md`
3. Vytvořit `TROUBLESHOOTING.md`
4. Vytvořit `REFERENCE.md`
5. Aktualizovat wiki

---

## Otázky k Rozhodnutí

1. **Zachovat `modules/` jako alias?**
   - **Navrhuji:** Ano, vytvořit symlinky nebo redirecty
   - **Důvod:** Zpětná kompatibilita

2. **Kde umístit troubleshooting?**
   - **Navrhuji:** `docs/TROUBLESHOOTING.md` (centrální)
   - **Alternativa:** Každý krok má vlastní troubleshooting sekci (kratší)

3. **Jak pojmenovat?**
   - **Navrhuji:** `phases/` a `steps/` (anglicky)
   - **Důvod:** Konzistence s aktuální strukturou

4. **Kdy implementovat?**
   - **Navrhuji:** Postupně, fáze po fázi
   - **Důvod:** Minimální riziko, testování po cestě

---

**Doporučení:** Začít s proof-of-concept (2-3 moduly), pak pokračovat postupně.

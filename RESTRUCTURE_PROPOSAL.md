# Návrh Restrukturalizace: Fáze → Kroky → Příkazy

**Cíl:** Transformovat moduly na čistou strukturu: **Fáze → Kroky → Příkazy**

**Filosofie:**
- **Fáze** = logická skupina kroků (např. "Příprava", "Nastavení disku", "Instalace systému")
- **Kroky** = jednotlivé moduly obsahující pouze příkazy (minimální text)
- **Příkazy** = konkrétní shell příkazy v krocích

---

## Navržená Struktura

### 1. Fáze (Phases)

**Fáze = soubor kroků jdoucích za sebou**

```
phases/
├── 00-PREPARATION.md          # Příprava (USB, Windows, formátování)
├── 01-DISK_SETUP.md           # Nastavení disku (partice, LUKS, Btrfs)
├── 02-SYSTEM_INSTALL.md       # Instalace systému (pacstrap, chroot)
├── 03-BASIC_CONFIG.md         # Základní konfigurace (locale, user, root)
├── 04-BOOTLOADER.md           # Bootloader (GRUB)
├── 05-NETWORK.md              # Síť (NetworkManager, WiFi, Bluetooth)
├── 06-AUDIO.md                # Audio (PipeWire)
├── 07-SECURITY.md             # Bezpečnost (SSH, UFW, fail2ban)
├── 08-LAPTOP_HARDWARE.md      # Laptop hardware (touchpad, webcam, IR, fingerprint)
└── 09-FINALIZE.md             # Finalizace (exit chroot, reboot)
```

**Struktura fáze:**
```markdown
# Phase: Preparation

**Purpose:** Prepare USB, install Windows, format disk

**Steps:**
1. `steps/PRE-01-create-arch-usb.md`
2. `steps/PRE-02-install-windows.md` (if dual boot)
3. `steps/PRE-03-format-disk.md` (if fresh install)

**Next:** `01-DISK_SETUP.md`
```

---

### 2. Kroky (Steps)

**Kroky = jednotlivé moduly s minimálním textem, jen příkazy**

```
steps/
├── PRE-01-create-arch-usb.md
├── PRE-02-install-windows.md
├── PRE-03-format-disk.md
├── 00-core-installation.md
├── 01-chroot.md
├── 02-locale.md
├── 03-root-password.md
├── 04-user-creation.md
├── 05-grub.md
├── 06-disk-partitioning.md
├── 07-luks-encryption.md
├── 08-btrfs-filesystem.md
├── 09-mount-partitions.md
├── 10-luks-keyfile-auto-unlock.md
├── 11-networkmanager.md
├── 12-wifi.md
├── 13-bluetooth.md
├── 14-audio.md
├── 15-exit-chroot.md
├── 17-laptop-touchpad.md
├── 18-laptop-webcam.md
├── 19-laptop-ir-camera.md
├── 20-laptop-fingerprint.md
├── 21-ssh-server.md
├── 22-ufw-firewall.md
└── 23-fail2ban.md
```

**Struktura kroku (minimální text, jen příkazy):**
```markdown
# Step: Core System Installation

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Network connected, partitions mounted at /mnt

## Commands

```bash
# Update mirrorlist (optional)
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup
reflector --country US,Germany,France --age 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist

# Install base system
pacstrap /mnt base base-devel linux linux-firmware linux-headers btrfs-progs dosfstools e2fsprogs networkmanager network-manager-applet vim nano git sudo man-db man-pages

# Generate fstab
genfstab -U /mnt >> /mnt/etc/fstab

# Verify
ls /mnt/bin /mnt/etc /mnt/usr
```

**VERIFICATION:**
```bash
ls -la /mnt/bin /mnt/etc /mnt/usr
cat /mnt/etc/fstab
```

**NEXT:** `01-chroot.md`
```

---

### 3. Příkazy (Commands)

**Příkazy = jednotlivé shell příkazy v krocích**

Každý příkaz by měl být:
- Samostatný (nebo logicky seskupený)
- S komentářem (co dělá)
- S očekávaným výstupem (pokud je důležité)

---

## Navržené Fáze

### Phase 00: PREPARATION
**Purpose:** Příprava před instalací

**Steps:**
1. `PRE-01-create-arch-usb.md` - Vytvoření Arch Linux USB
2. `PRE-02-install-windows.md` - Instalace Windows (dual boot)
3. `PRE-03-format-disk.md` - Formátování disku (fresh install)

**When:** Před bootováním z USB

---

### Phase 01: DISK_SETUP
**Purpose:** Nastavení disku a filesystému

**Steps:**
1. `06-disk-partitioning.md` - Vytvoření particí
2. `07-luks-encryption.md` - Šifrování (pokud LUKS)
3. `08-btrfs-filesystem.md` - Btrfs filesystém
4. `09-mount-partitions.md` - Mount (pokud ne Btrfs)

**When:** Po bootování z USB, před instalací systému

---

### Phase 02: SYSTEM_INSTALL
**Purpose:** Instalace základního systému

**Steps:**
1. `00-core-installation.md` - Pacstrap instalace
2. `01-chroot.md` - Vstup do chroot

**When:** Po nastavení disku

---

### Phase 03: BASIC_CONFIG
**Purpose:** Základní konfigurace systému

**Steps:**
1. `02-locale.md` - Locale a timezone
2. `03-root-password.md` - Root heslo
3. `04-user-creation.md` - Vytvoření uživatele

**When:** V chroot prostředí

---

### Phase 04: BOOTLOADER
**Purpose:** Instalace bootloaderu

**Steps:**
1. `05-grub.md` - GRUB instalace
2. `10-luks-keyfile-auto-unlock.md` - Auto-unlock (pokud LUKS)

**When:** V chroot prostředí, před exit

---

### Phase 05: NETWORK
**Purpose:** Síťová konfigurace

**Steps:**
1. `11-networkmanager.md` - NetworkManager
2. `12-wifi.md` - WiFi (pokud WiFi)
3. `13-bluetooth.md` - Bluetooth (pokud Bluetooth)

**When:** V chroot nebo po prvním bootu

---

### Phase 06: AUDIO
**Purpose:** Audio konfigurace

**Steps:**
1. `14-audio.md` - PipeWire audio

**When:** V chroot nebo po prvním bootu

---

### Phase 07: SECURITY
**Purpose:** Bezpečnostní konfigurace

**Steps:**
1. `21-ssh-server.md` - SSH server
2. `22-ufw-firewall.md` - UFW firewall
3. `23-fail2ban.md` - Fail2ban

**When:** V chroot nebo po prvním bootu

---

### Phase 08: LAPTOP_HARDWARE
**Purpose:** Laptop hardware konfigurace

**Steps:**
1. `17-laptop-touchpad.md` - Touchpad
2. `18-laptop-webcam.md` - Webcam
3. `19-laptop-ir-camera.md` - IR kamera
4. `20-laptop-fingerprint.md` - Fingerprint reader

**When:** Po prvním bootu (ne v chroot)

---

### Phase 09: FINALIZE
**Purpose:** Finalizace instalace

**Steps:**
1. `15-exit-chroot.md` - Exit chroot, unmount, reboot

**When:** Po dokončení všech konfigurací

---

## Transformace Modulů na Kroky

### Příklad: `00-core-installation.md`

**PŘED (aktuální):**
- Dlouhé vysvětlení
- Prerequisites checklist
- Troubleshooting
- Verification sekce
- ~140 řádků

**PO (navrhované):**
- Minimální text (ENVIRONMENT, PREREQUISITES)
- Jen příkazy
- Krátká verification
- ~30 řádků

---

## Struktura Složek

```
arch-linux/
├── phases/                    # Fáze (soubory kroků)
│   ├── 00-PREPARATION.md
│   ├── 01-DISK_SETUP.md
│   ├── 02-SYSTEM_INSTALL.md
│   └── ...
├── steps/                     # Kroky (příkazy)
│   ├── PRE-01-create-arch-usb.md
│   ├── PRE-02-install-windows.md
│   ├── 00-core-installation.md
│   └── ...
├── commands/                  # (volitelné) Izolované příkazy
│   └── (pokud by bylo potřeba)
├── docs/                      # Dokumentace (zůstává)
│   ├── INSTALLATION-SCENARIOS.md
│   ├── MODULE_INDEX.md
│   └── ...
└── wiki/                      # Wiki (zůstává)
    └── ...
```

---

## Výhody Této Struktury

1. **Čistá separace:** Fáze → Kroky → Příkazy
2. **Minimální text:** Kroky obsahují jen příkazy
3. **Logické skupiny:** Fáze seskupují související kroky
4. **Snadná navigace:** Jasná hierarchie
5. **Automatizace:** Snadné generování procedur z fází

---

## Implementační Plán

### Krok 1: Vytvořit strukturu složek
- `phases/` - pro fáze
- `steps/` - pro kroky (přesunout z `modules/`)

### Krok 2: Transformovat moduly na kroky
- Odstranit dlouhé vysvětlení
- Zanechat jen příkazy
- Přidat minimální metadata (ENVIRONMENT, PREREQUISITES)

### Krok 3: Vytvořit fáze
- Každá fáze = seznam kroků
- Fáze obsahují odkazy na kroky
- Fáze obsahují "Next" odkaz na další fázi

### Krok 4: Aktualizovat dokumentaci
- `INSTALLATION-SCENARIOS.md` - odkazovat na fáze místo modulů
- `MODULE_INDEX.md` - přejmenovat na `STEP_INDEX.md`
- Wiki - aktualizovat odkazy

---

## Příklad Fáze

```markdown
# Phase 01: Disk Setup

**Purpose:** Configure disk partitions, encryption, and filesystem

**Prerequisites:**
- Booted from Arch Linux Live USB
- Disk identified

**Steps (in order):**

1. **[Disk Partitioning](../steps/06-disk-partitioning.md)**
   - Create EFI, root, and swap partitions
   - Choose single boot or dual boot layout

2. **[LUKS Encryption](../steps/07-luks-encryption.md)** (if using encryption)
   - Encrypt root and swap partitions
   - Set strong passphrase

3. **[Btrfs Filesystem](../steps/08-btrfs-filesystem.md)**
   - Create Btrfs on encrypted or unencrypted partition
   - Create subvolumes (@, @home, @log, @cache, @snapshots)

4. **[Mount Partitions](../steps/09-mount-partitions.md)** (if not using Btrfs)
   - Mount partitions for installation

**Verification:**
```bash
lsblk -f
mount | grep /mnt
```

**Next:** [Phase 02: System Install](02-SYSTEM_INSTALL.md)
```

---

## Příklad Kroku (Transformovaný)

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
# Check critical directories exist
ls -la /mnt/bin /mnt/etc /mnt/usr

# Check fstab was generated
cat /mnt/etc/fstab
```

**NEXT:** `01-chroot.md`
```

---

## Otázky k Rozhodnutí

1. **Zachovat troubleshooting?**
   - **Navrhuji:** Ne, přesunout do samostatného `TROUBLESHOOTING.md`
   - **Alternativa:** Ano, ale jako samostatná sekce na konci kroku

2. **Zachovat dlouhé vysvětlení?**
   - **Navrhuji:** Ne, přesunout do `docs/` jako reference dokumentaci
   - **Alternativa:** Ano, ale jako samostatná sekce

3. **Jak pojmenovat?**
   - **Navrhuji:** `phases/` a `steps/` (anglicky, konzistentní)
   - **Alternativa:** `faze/` a `kroky/` (česky)

4. **Kompatibilita s aktuální strukturou?**
   - **Navrhuji:** Postupná migrace, zachovat `modules/` jako alias
   - **Alternativa:** Kompletní přepis najednou

---

**Doporučení:** Začít s transformací 2-3 modulů jako proof-of-concept, pak pokračovat postupně.

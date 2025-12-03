# Frequently Asked Questions (FAQ)

**Purpose:** Answers to the most common questions about installing and configuring Arch Linux using this modular system.

**Quick Links:**
- [Main Index](index.md) - Choose your hardware configuration
- [Installation Checklist](installation-checklist.md) - Track your progress
- [Troubleshooting](troubleshooting.md) - If you encounter problems

---

## General Questions

### Q: Kde začít s instalací?

**A:** Začněte na [index.md](index.md) - hlavní stránka s odkazy na všechny konfigurace. Vyberte svůj hardware typ (Desktop/Laptop, Intel/AMD, GPU) a následujte doporučené fáze.

---

### Q: Jak dlouho trvá instalace?

**A:** Záleží na vašem scénáři:
- **Minimální instalace:** 1-2 hodiny
- **Standardní instalace:** 2-4 hodiny
- **Plná instalace (šifrování + desktop):** 3-6 hodin

Podrobné odhady najdete v [time-estimates.md](time-estimates.md).

---

### Q: Potřebuji předchozí zkušenosti s Linuxem?

**A:** 
- **Základní znalosti:** Doporučeno znát základní příkazy (cd, ls, nano)
- **Pokročilé:** Pro šifrování a pokročilou konfiguraci je užitečná zkušenost
- **Začátečník:** Můžete začít s minimální instalací a postupně se učit

---

### Q: Je to bezpečné pro začátečníky?

**A:** 
- **Ano, pokud:** Pečlivě následujete instrukce a máte zálohu dat
- **Pozor:** Instalace může smazat data na disku - vždy zálohujte!
- **Doporučení:** Začněte s minimální instalací bez šifrování

---

### Q: Co když udělám chybu?

**A:**
- Většina chyb je opravitelná - viz [troubleshooting.md](troubleshooting.md)
- Nejčastější chyby najdete v [common-mistakes.md](common-mistakes.md)
- Můžete vždy začít znovu s Live USB

---

## Installation Type Questions

### Q: Můžu mít Windows a Arch Linux na stejném počítači?

A: Ano, viz [Dual Boot scénáře](installation-scenarios.md#scenario-1-dual-boot-arch-linux-windows)

**Důležité:**
- Nainstalujte Windows PRVNÍ
- Vypněte Windows Fast Startup
- Vypněte BitLocker (pokud je zapnutý)

---

### Q: Mám použít šifrování (LUKS)?

**A:** 
- **Ano, pokud:** Chcete chránit data (laptopy, citlivá data)
- **Ne, pokud:** Chcete rychlejší boot a jednodušší instalaci
- **Doporučení:** Pro laptopy vždy šifrování, pro desktop volitelné

Více v [LUKS Encryption Module](modules/luks-encryption.md).

---

### Q: Jaký filesystem použít - Btrfs nebo ext4?

**A:**
- **Btrfs:** Doporučeno - podpora snapshotů, komprese, moderní features
- **ext4:** Tradiční, stabilní, jednodušší
- **Doporučení:** Pro většinu uživatelů Btrfs

Porovnání najdete v [comparison-guide.md](comparison-guide.md).

---

## Hardware Questions

### Q: Funguje to s mým hardwarem?

**A:**
- **Intel/AMD CPU:** ✅ Ano, plně podporováno
- **NVIDIA GPU:** ✅ Ano, vyžaduje ovladače - viz [NVIDIA Drivers](modules/nvidia-drivers.md)
- **AMD GPU:** ✅ Ano, často funguje out-of-the-box - viz [AMD Drivers](modules/amd-drivers.md)
- **Laptop hardware:** ✅ Touchpad, webcam, fingerprint - viz [Hardware Phase](phases/08-hardware.md)

---

### Q: Mám NVIDIA GPU - co potřebuji?

**A:**
1. Po instalaci nainstalujte [NVIDIA Drivers](modules/nvidia-drivers.md)
2. Použijte Xorg (Wayland má omezenou podporu)
3. Po instalaci ovladačů restartujte

---

### Q: Mám laptop - co navíc potřebuji?

**A:**
- **WiFi:** [WiFi Module](modules/wifi.md)
- **Touchpad:** [Touchpad Module](modules/touchpad.md)
- **Webcam:** [Webcam Module](modules/webcam.md)
- **Fingerprint:** [Fingerprint Module](modules/fingerprint.md) (pokud máte)

Viz [Hardware Phase](phases/08-hardware.md).

---

## Desktop Environment Questions

### Q: Jaký desktop environment zvolit?

**A:**
- **GNOME:** Moderní, uživatelsky přívětivý, střední náročnost
- **KDE Plasma:** Vysoce přizpůsobitelný, více možností
- **XFCE:** Lehký, vhodný pro starší hardware
- **i3/Hyprland:** Pro pokročilé uživatele, tiling window manager

Porovnání v [comparison-guide.md](comparison-guide.md).

---

### Q: Xorg nebo Wayland?

**A:**
- **Xorg:** Zralý, plná podpora NVIDIA, stabilní
- **Wayland:** Moderní, lepší bezpečnost, omezená NVIDIA podpora
- **Doporučení:** NVIDIA → Xorg, ostatní → Wayland

Více v [comparison-guide.md](comparison-guide.md).

---

## Network Questions

### Q: Jak se připojit k WiFi během instalace?

**A:**
1. V Live USB použijte `iwctl`:
   ```bash
   iwctl
   station wlan0 scan
   station wlan0 get-networks
   station wlan0 connect <SSID>
   ```

2. Po instalaci použijte NetworkManager:
   ```bash
   nmtui  # interaktivní rozhraní
   ```

Viz [WiFi Module](modules/wifi.md).

---

### Q: Ne funguje mi síť po instalaci

**A:**
1. Zkontrolujte NetworkManager:
   ```bash
   systemctl status NetworkManager
   systemctl start NetworkManager
   ```

2. Pro WiFi nainstalujte balíčky:
   ```bash
   pacman -S networkmanager wireless_tools wpa_supplicant
   ```

- Troubleshooting - Network → [troubleshooting.md#network-issues](troubleshooting.md#network-issues)

---

## Security Questions

### Q: Je potřeba firewall?

**A:**
- **Ano, doporučeno:** Zvláště pokud máte SSH nebo jiné služby
- **UFW:** Jednoduchý firewall - viz [UFW Module](modules/ufw-firewall.md)
- **Fail2ban:** Ochrana proti brute force - viz [Fail2ban Module](modules/fail2ban.md)

---

### Q: Jak zabezpečit SSH?

**A:**
1. Změňte výchozí port (např. 1991)
2. Zakázat root login
3. Použít SSH keys místo hesla
4. Zapnout Fail2ban

Viz [SSH Server Module](modules/ssh-server.md).

---

## Post-Installation Questions

### Q: Co dělat po první boot?

**A:**
1. Připojit se k síti
2. Aktualizovat systém: `sudo pacman -Syu`
3. Nainstalovat GPU ovladače (pokud potřebné)
4. Nainstalovat desktop environment
5. Nainstalovat základní aplikace

Kompletní checklist v [post-install-checklist.md](post-install-checklist.md).

---

### Q: Jak aktualizovat systém?

**A:**
```bash
sudo pacman -Syu  # Aktualizace všech balíčků
```

**Doporučení:**
- Aktualizujte pravidelně
- Před aktualizací zkontrolujte [Arch Linux News](https://archlinux.org/news/)

---

### Q: Jak zálohovat systém?

**A:**
- **Timeshift:** Snapshot systém - viz [Timeshift Module](modules/timeshift.md)
- **Btrfs snapshots:** Pokud používáte Btrfs
- **Manuální záloha:** Zkopírovat důležitá data

Viz [Backup & Recovery Guide](backup-recovery.md).

---

## Troubleshooting Questions

### Q: GRUB se nezobrazuje

**A:**
1. Zkontrolujte instalaci GRUB: `ls -la /boot/EFI/GRUB/`
2. Reinstalujte GRUB z Live USB
3. Zkontrolujte BIOS/UEFI nastavení

- Troubleshooting - Boot Issues → [troubleshooting.md#boot-issues](troubleshooting.md#boot-issues)

---

### Q: Systém se nespustí po instalaci

**A:**
1. Boot z Live USB
2. Zkontrolujte UUID v `/etc/default/grub`
3. Zkontrolujte mounty: `mount | grep /mnt`
4. Regenerujte GRUB: `grub-mkconfig -o /boot/grub/grub.cfg`

Viz [Troubleshooting Guide](troubleshooting.md).

---

### Q: Zapomněl jsem LUKS passphrase

**A:**
- **Bohužel:** Bez passphrase nelze data obnovit
- **Prevence:** Zálohujte recovery key
- **Pokud máte keyfile:** Můžete použít keyfile místo passphrase

Viz [LUKS Troubleshooting](modules/luks-encryption.md#troubleshooting).

---

## Advanced Questions

### Q: Můžu přizpůsobit instalační proces?

**A:**
- **Ano!** Tento systém je modulární - vyberte si moduly, které potřebujete
- Vytvořte vlastní proceduru podle [generate-procedure.md](generate-procedure.md)
- Kombinujte moduly podle potřeby

---

### Q: Jak přispět do tohoto repozitáře?

**A:**
- Viz [contributing.md](contributing.md) pro pokyny
- Přidejte nové moduly
- Vylepšete existující dokumentaci
- Nahlaste problémy

---

### Q: Kde najdu více informací?

**A:**
- **ArchWiki:** https://wiki.archlinux.org/ - Kompletní dokumentace
- **Arch Linux Forums:** https://bbs.archlinux.org/ - Komunita
- **Tento repozitář:** Všechny moduly a fáze

---

## Still Have Questions?

- Check [Troubleshooting Guide](troubleshooting.md)
- Review [Common Mistakes](common-mistakes.md)
- Search [ArchWiki](https://wiki.archlinux.org/)
- Ask on [Arch Linux Forums](https://bbs.archlinux.org/)

---

**Back to:** [Main Index](index.md) | [Repository Root](README.md)

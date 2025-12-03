# Common Mistakes and How to Avoid Them

**Purpose:** List of the most common mistakes during Arch Linux installation and how to prevent or fix them.

**Quick Links:**
- [Installation Checklist](installation-checklist.md) - Track your progress
- [Troubleshooting](troubleshooting.md) - Detailed troubleshooting
- [FAQ](faq.md) - Frequently asked questions

---

## ⚠️ Critical Mistakes

### Mistake 1: Not Backing Up Data

**Problem:** Installing on a disk with important data without backup.

**Consequence:** Permanent data loss if something goes wrong.

**Solution:**
- ✅ Always backup important data before installation
- ✅ Use external drive or cloud backup
- ✅ Verify backup is complete and accessible
- ✅ Test backup restoration (if possible)

**Prevention:** Make backup part of your pre-installation checklist.

---

### Mistake 2: Installing on Wrong Disk

**Problem:** Accidentally installing on the wrong disk/partition.

**Consequence:** Data loss on the wrong disk.

**Solution:**
- ✅ Always verify disk with `lsblk` before partitioning
- ✅ Double-check disk identifier (`/dev/sdX` or `/dev/nvme0n1`)
- ✅ Unmount all other disks before installation
- ✅ Use `fdisk -l` to list all disks

**Prevention:** 
```bash
# Always check before partitioning
lsblk
fdisk -l
# Verify which disk is your target
```

---

### Mistake 3: Wrong UUID in GRUB

**Problem:** Using wrong UUID in GRUB configuration.

**Consequence:** System won't boot.

**Solution:**
- ✅ Always use `blkid` to get correct UUIDs
- ✅ Copy UUID directly (don't type manually)
- ✅ Verify UUID matches partition: `lsblk -f`
- ✅ Test GRUB configuration before rebooting

**Prevention:**
```bash
# Get UUID
blkid
# Copy UUID exactly
# Verify in /etc/default/grub
```

---

### Mistake 4: Forgetting LUKS Passphrase

**Problem:** Setting LUKS passphrase but forgetting it.

**Consequence:** Cannot access encrypted data (permanent loss).

**Solution:**
- ✅ Write down passphrase in secure location
- ✅ Use password manager
- ✅ Test passphrase before finalizing
- ✅ Consider using keyfile (in addition to passphrase)

**Prevention:** 
- Store passphrase securely
- Test passphrase: `cryptsetup open --test-passphrase /dev/sdXY`

---

## 🔧 Installation Mistakes

### Mistake 5: Skipping Network Test

**Problem:** Not testing network before `pacstrap`.

**Consequence:** Installation fails or is very slow.

**Solution:**
- ✅ Always test: `ping archlinux.org`
- ✅ Verify network before starting installation
- ✅ Check if WiFi needs configuration

**Prevention:**
```bash
# Always test before pacstrap
ping archlinux.org
ip link  # Check network interface
```

---

### Mistake 6: Not Updating Mirror List

**Problem:** Using slow or outdated mirrors.

**Consequence:** Very slow installation or package errors.

**Solution:**
- ✅ Update mirror list before `pacstrap`
- ✅ Use `reflector` to get fast mirrors
- ✅ Test mirror speed

**Prevention:**
```bash
# Update mirrors before installation
reflector --country "Czech Republic" --latest 10 --sort rate --save /etc/pacman.d/mirrorlist
```

---

### Mistake 7: Insufficient Disk Space

**Problem:** Not leaving enough space for installation.

**Consequence:** Installation fails or system runs out of space.

**Solution:**
- ✅ Check available space: `df -h`
- ✅ Ensure at least 10-20 GB for root partition
- ✅ Plan partition sizes before installation

**Prevention:**
- Minimum 20 GB for root (recommended 50+ GB)
- Check space: `df -h /mnt`

---

### Mistake 8: Wrong Partition Mount Order

**Problem:** Mounting partitions in wrong order.

**Consequence:** Installation fails or system won't boot.

**Solution:**
- ✅ Always mount root first: `mount /dev/sdXY /mnt`
- ✅ Then mount EFI: `mount /dev/sdXY /mnt/boot`
- ✅ For Btrfs, mount subvolumes correctly

**Prevention:**
```bash
# Correct order
mount /dev/sdXY /mnt
mount /dev/sdXY /mnt/boot
# Then pacstrap
```

---

## 🖥️ Configuration Mistakes

### Mistake 9: Not Setting Locale

**Problem:** Forgetting to set locale and timezone.

**Consequence:** Wrong language, date/time incorrect.

**Solution:**
- ✅ Always set locale: `locale-gen`
- ✅ Set timezone: `timedatectl set-timezone`
- ✅ Verify: `locale` and `timedatectl status`

**Prevention:** Follow [Locale Module](modules/locale.md) carefully.

---

### Mistake 10: Forgetting Root Password

**Problem:** Not setting root password or forgetting it.

**Consequence:** Cannot perform administrative tasks.

**Solution:**
- ✅ Always set root password: `passwd`
- ✅ Write it down securely
- ✅ Test password before exiting chroot

**Prevention:** Set password immediately after user creation.

---

### Mistake 11: Not Creating User with Sudo

**Problem:** Forgetting to add user to `wheel` group.

**Consequence:** Cannot use `sudo`, limited functionality.

**Solution:**
- ✅ Add user to wheel: `usermod -aG wheel <username>`
- ✅ Configure sudo: `visudo`
- ✅ Test sudo: `sudo whoami`

**Prevention:** Follow [User Creation Module](modules/user-creation.md).

---

### Mistake 12: Wrong GRUB Configuration

**Problem:** Incorrect GRUB configuration (wrong UUID, missing parameters).

**Consequence:** System won't boot.

**Solution:**
- ✅ Verify UUID: `blkid`
- ✅ Check LUKS parameters (if using encryption)
- ✅ Check Btrfs subvolume flags (if using Btrfs)
- ✅ Regenerate GRUB: `grub-mkconfig -o /boot/grub/grub.cfg`

**Prevention:** Follow [GRUB Module](modules/grub.md) carefully.

---

## 🌐 Network Mistakes

### Mistake 13: Not Enabling NetworkManager

**Problem:** Forgetting to enable NetworkManager service.

**Consequence:** No network after reboot.

**Solution:**
- ✅ Enable NetworkManager: `systemctl enable NetworkManager`
- ✅ Start it: `systemctl start NetworkManager`
- ✅ Verify: `systemctl status NetworkManager`

**Prevention:** Enable NetworkManager before exiting chroot.

---

### Mistake 14: WiFi Not Working After Boot

**Problem:** Not installing WiFi packages.

**Consequence:** Cannot connect to WiFi after installation.

**Solution:**
- ✅ Install WiFi packages: `pacman -S networkmanager wireless_tools wpa_supplicant`
- ✅ Enable NetworkManager
- ✅ Configure WiFi: `nmtui` or `nmcli`

**Prevention:** Install WiFi packages during installation phase.

---

## 🔒 Security Mistakes

### Mistake 15: Using Default SSH Port

**Problem:** Leaving SSH on default port 22.

**Consequence:** Increased risk of brute force attacks.

**Solution:**
- ✅ Change SSH port (e.g., 1991)
- ✅ Disable root login
- ✅ Use SSH keys instead of passwords
- ✅ Enable Fail2ban

**Prevention:** Follow [SSH Server Module](modules/ssh-server.md).

---

### Mistake 16: Not Setting Up Firewall

**Problem:** No firewall configured.

**Consequence:** System exposed to network attacks.

**Solution:**
- ✅ Install UFW: `pacman -S ufw`
- ✅ Configure UFW: `ufw enable`
- ✅ Allow necessary ports only

**Prevention:** Set up firewall as part of security phase.

---

## 💻 Post-Installation Mistakes

### Mistake 17: Not Updating System

**Problem:** Not updating system after installation.

**Consequence:** Security vulnerabilities, missing features.

**Solution:**
- ✅ Always update: `sudo pacman -Syu`
- ✅ Update before installing additional software
- ✅ Check Arch Linux news for breaking changes

**Prevention:** Make updating first step after first boot.

---

### Mistake 18: Installing GPU Drivers Too Late

**Problem:** Trying to use desktop before installing GPU drivers.

**Consequence:** Poor performance, display issues.

**Solution:**
- ✅ Install GPU drivers before desktop environment
- ✅ NVIDIA: Install `nvidia` package
- ✅ AMD: Usually works, but install `mesa` if needed

**Prevention:** Install GPU drivers as first step after network.

---

### Mistake 19: Not Testing Desktop Environment

**Problem:** Installing desktop but not testing it works.

**Consequence:** Desktop doesn't work, hard to troubleshoot.

**Solution:**
- ✅ Test desktop immediately after installation
- ✅ Verify display works
- ✅ Test basic applications

**Prevention:** Test desktop before installing other software.

---

## 🎯 General Prevention Tips

### Always Verify
- ✅ Check commands before executing
- ✅ Verify disk/partition identifiers
- ✅ Test network before installation
- ✅ Verify UUIDs before using them

### Use Checklists
- ✅ Follow [Installation Checklist](installation-checklist.md)
- ✅ Check off items as you complete them
- ✅ Don't skip verification steps

### Read Documentation
- ✅ Read module instructions carefully
- ✅ Check prerequisites
- ✅ Follow steps in order

### Test Before Proceeding
- ✅ Test network: `ping archlinux.org`
- ✅ Test mounts: `mount | grep /mnt`
- ✅ Test chroot: `arch-chroot /mnt`
- ✅ Test services: `systemctl status <service>`

### Keep Notes
- ✅ Write down important information (UUIDs, passwords)
- ✅ Note any custom configurations
- ✅ Document troubleshooting steps

---

## 🆘 If You Made a Mistake

### Don't Panic
- Most mistakes are fixable
- You can always start over with Live USB
- Check [Troubleshooting Guide](troubleshooting.md)

### Common Fixes
1. **Boot from Live USB**
2. **Mount partitions**
3. **Chroot into system**
4. **Fix the issue**
5. **Reboot**

### Get Help
- [Troubleshooting Guide](troubleshooting.md)
- [ArchWiki](https://wiki.archlinux.org/)
- [Arch Linux Forums](https://bbs.archlinux.org/)

---

**Remember:** Everyone makes mistakes. The key is learning from them and being careful next time!

---

**Back to:** [Main Index](index.md) | [Repository Root](README.md)

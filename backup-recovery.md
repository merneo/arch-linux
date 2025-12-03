# Backup & Recovery Guide

**Purpose:** Guide for backing up your Arch Linux system and recovering from issues.

**Quick Links:**
- [Timeshift Module](modules/timeshift.md) - System snapshot tool
- [Troubleshooting](troubleshooting.md) - If recovery needed
- [Post-Installation Checklist](post-install-checklist.md) - Setup verification

---

## Backup Strategies

### Before Installation

**Always backup before installing:**
- ✅ Important data to external drive
- ✅ System configuration files
- ✅ Application settings
- ✅ Verify backup is complete

---

## System Backup Methods

### Method 1: Timeshift (Recommended)

**Best for:** System snapshots, easy recovery

1. **Install Timeshift:**
   ```bash
   sudo pacman -S timeshift
   ```

2. **Create snapshot:**
   ```bash
   sudo timeshift --create --comments "Before major update"
   ```

3. **Schedule automatic snapshots:**
   ```bash
   sudo timeshift --create --schedule
   ```

**See:** [Timeshift Module](modules/timeshift.md)

---

### Method 2: Btrfs Snapshots

**Best for:** Btrfs filesystem users

1. **Create snapshot:**
   ```bash
   sudo btrfs subvolume snapshot / /snapshots/$(date +%Y%m%d)
   ```

2. **List snapshots:**
   ```bash
   sudo btrfs subvolume list /
   ```

3. **Delete snapshot:**
   ```bash
   sudo btrfs subvolume delete /snapshots/<snapshot-name>
   ```

**Advantages:**
- Instant creation
- Minimal disk space
- Easy rollback

---

### Method 3: Full Disk Image

**Best for:** Complete system backup

1. **Create disk image:**
   ```bash
   # Boot from Live USB
   sudo dd if=/dev/sdX of=/path/to/backup.img bs=4M status=progress
   ```

2. **Compress:**
   ```bash
   gzip backup.img
   ```

**Note:** Requires external storage, takes time.

---

### Method 4: File-Level Backup

**Best for:** Important files and configurations

1. **Backup home directory:**
   ```bash
   tar -czf home-backup.tar.gz /home/
   ```

2. **Backup configuration:**
   ```bash
   tar -czf etc-backup.tar.gz /etc/
   ```

3. **Backup package list:**
   ```bash
   pacman -Q > package-list.txt
   ```

---

## Recovery Procedures

### Recovery from Timeshift Snapshot

1. **Boot from Live USB**

2. **Mount partitions:**
   ```bash
   mount /dev/sdXY /mnt
   mount /dev/sdXY /mnt/boot  # If separate
   ```

3. **Install Timeshift:**
   ```bash
   arch-chroot /mnt
   pacman -S timeshift
   ```

4. **Restore snapshot:**
   ```bash
   timeshift --restore
   ```

- Timeshift Module - Restore ([Timeshift](modules/timeshift.md#step-3-command-line-usage-optional))

---

### Recovery from Btrfs Snapshot

1. **Boot from Live USB**

2. **Mount Btrfs:**
   ```bash
   mount /dev/sdXY /mnt
   ```

3. **List snapshots:**
   ```bash
   btrfs subvolume list /mnt
   ```

4. **Restore snapshot:**
   ```bash
   # Move current root
   mv /mnt/@ /mnt/@.broken
   # Restore snapshot
   btrfs subvolume snapshot /mnt/snapshots/<snapshot> /mnt/@
   ```

5. **Reboot**

---

### Recovery from GRUB

If system won't boot but GRUB works:

1. **In GRUB menu, press 'e' to edit**

2. **Add to kernel line:**
   ```
   systemd.unit=rescue.target
   ```

3. **Boot to rescue mode**

4. **Fix the issue:**
   ```bash
   # Mount if needed
   mount -o remount,rw /
   # Fix configuration
   # Reboot
   ```

---

### Recovery from Live USB

1. **Boot from Arch Linux Live USB**

2. **Mount partitions:**
   ```bash
   mount /dev/sdXY /mnt
   mount /dev/sdXY /mnt/boot  # If separate
   # For Btrfs:
   mount -o subvol=@ /dev/sdXY /mnt
   ```

3. **Chroot into system:**
   ```bash
   arch-chroot /mnt
   ```

4. **Fix issues:**
   - Reinstall packages
   - Fix configuration
   - Restore from backup

5. **Reboot**

---

## Data Recovery

### Recovering Deleted Files

1. **Install recovery tools:**
   ```bash
   pacman -S testdisk photorec
   ```

2. **Use testdisk:**
   ```bash
   sudo testdisk
   ```

3. **Use photorec:**
   ```bash
   sudo photorec
   ```

**Note:** Stop using disk immediately after deletion!

---

### Recovering from Corrupted Filesystem

1. **Boot from Live USB**

2. **Check filesystem:**
   ```bash
   # For ext4
   fsck /dev/sdXY
   
   # For Btrfs
   btrfs check /dev/sdXY
   ```

3. **Repair if possible:**
   ```bash
   # ext4
   fsck -y /dev/sdXY
   
   # Btrfs (read-only check first)
   btrfs check --read-only /dev/sdXY
   ```

---

## LUKS Recovery

### Recovering Encrypted Volume

1. **If passphrase forgotten:**
   - Cannot recover without passphrase
   - Use keyfile if available

2. **If keyfile lost:**
   ```bash
   # Boot from Live USB
   cryptsetup open /dev/sdXY cryptroot
   # If passphrase works, recreate keyfile
   ```

3. **Backup LUKS header:**
   ```bash
   cryptsetup luksHeaderBackup /dev/sdXY --header-backup-file luks-header-backup
   ```

4. **Restore LUKS header:**
   ```bash
   cryptsetup luksHeaderRestore /dev/sdXY --header-backup-file luks-header-backup
   ```

---

## Backup Best Practices

### Regular Backups

1. **Schedule automatic snapshots:**
   - Daily for active systems
   - Weekly for stable systems
   - Before major updates

2. **Keep multiple backups:**
   - Recent snapshots on disk
   - Older backups on external drive
   - Critical data in cloud

3. **Test backups:**
   - Periodically test restore
   - Verify backup integrity
   - Document recovery procedures

---

### What to Backup

**Essential:**
- ✅ Home directory (`/home/`)
- ✅ Configuration files (`/etc/`)
- ✅ Package list (`pacman -Q > packages.txt`)
- ✅ AUR packages list

**Optional:**
- System snapshots (Timeshift/Btrfs)
- Application data
- Custom scripts

**Don't backup:**
- `/tmp/`, `/var/tmp/`
- Cache directories
- Temporary files

---

## Recovery Checklist

### Before Recovery

- [ ] Identify the problem
- [ ] Determine recovery method needed
- [ ] Have Live USB ready
- [ ] Know your partition layout
- [ ] Have backup available (if possible)

### During Recovery

- [ ] Boot from Live USB
- [ ] Mount partitions correctly
- [ ] Chroot if needed
- [ ] Fix the issue
- [ ] Verify fix works
- [ ] Reboot and test

### After Recovery

- [ ] Verify system works
- [ ] Update system
- [ ] Create new backup
- [ ] Document what happened
- [ ] Prevent future issues

---

## Emergency Recovery

### System Won't Boot

1. **Boot from Live USB**
2. **Mount and chroot**
3. **Check logs:**
   ```bash
   journalctl -b
   dmesg
   ```
4. **Fix configuration**
5. **Reboot**

### Can't Access System

1. **Use GRUB rescue mode**
2. **Or boot from Live USB**
3. **Mount and fix**

---

## Prevention

### Regular Maintenance

- ✅ Keep system updated
- ✅ Regular backups
- ✅ Monitor disk space
- ✅ Check system logs
- ✅ Test recovery procedures

### Before Major Changes

- ✅ Create snapshot
- ✅ Backup important data
- ✅ Document changes
- ✅ Have recovery plan

---

**Remember:** Prevention is better than recovery. Regular backups save time and data!

---

**Back to:** [Main Index](index.md) | [Repository Root](README.md)

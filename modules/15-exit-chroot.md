# Module: Exit Chroot and Reboot

**Purpose:** Exit chroot environment, unmount partitions, and reboot system

**Prerequisites:**
- All configuration modules completed
- System ready for first boot

**Time:** 5 minutes

---

## Step 1: Exit Chroot Environment

```bash
exit
```

**Prompt changes back to:**
```
root@archiso ~ #
```

**SUCCESS:** You are now back in Arch Linux live USB environment

---

## Step 2: Unmount All Partitions

```bash
# Unmount all mounted filesystems recursively
umount -R /mnt
```

**If error "target is busy":**
```bash
# Force unmount
umount -lR /mnt
```

---

## Step 3: Close Encrypted Volumes (if used)

```bash
# Close encrypted swap
cryptsetup close cryptswap

# Close encrypted root
cryptsetup close cryptroot
```

**Verify all closed:**
```bash
ls /dev/mapper/

# Should show only:
# control
```

---

## Step 4: Reboot System

```bash
reboot
```

**Remove USB drive** when system shuts down (before it boots again).

---

**SUCCESS:** System ready for first boot

**After reboot:**
- GRUB menu appears
- Select Arch Linux
- Enter LUKS passphrase (if not using keyfile)
- Login with created user
- Continue with post-installation configuration

---

**Installation Complete!**

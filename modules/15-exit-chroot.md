# Module: Exit Chroot and Reboot

**Purpose:** Exit chroot environment, unmount partitions, and reboot the system into your newly installed Arch Linux. This marks the transition from the live environment to the configured system. For more details on the final steps, refer to the [ArchWiki Installation Guide#Unmount the root partition](https://wiki.archlinux.org/title/Installation_guide#Unmount_the_root_partition).

**Prerequisites:**
- All configuration modules completed
- System ready for first boot

**Time:** 5 minutes

**ENVIRONMENT:** Chroot (root@archiso /)# → Live USB (root@archiso)

---

## Step 1: Exit Chroot Environment

Executing `exit` from the chroot environment returns you to the Arch Linux live USB session. For an explanation of the `chroot` environment, refer to [ArchWiki: Chroot](https://wiki.archlinux.org/title/Chroot).

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

It is crucial to unmount all partitions from `/mnt` before rebooting to ensure data integrity. The `umount -R` command recursively unmounts filesystems. For more details, refer to the [ArchWiki on Mount](https://wiki.archlinux.org/title/Mount#Unmounting).

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

If LUKS encrypted volumes were opened, they should be explicitly closed before rebooting. The `cryptsetup close` command detaches the decrypted device from the mapper. For more details, refer to the [ArchWiki on dm-crypt/Device encryption](https://wiki.archlinux.org/title/Dm-crypt/Device_encryption#Closing_the_device).

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

The `reboot` command initiates a system restart. This is the final step to boot into your newly installed Arch Linux system. For more details, consult `man reboot` or refer to the [ArchWiki on systemd#Power_management](https://wiki.archlinux.org/title/Systemd#Power_management).

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

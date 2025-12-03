# Step: Exit Chroot and Reboot

**Purpose:** Exit chroot environment, unmount partitions, and reboot the system. For more details on the final steps, refer to the [ArchWiki Installation Guide#Unmount the root partition](https://wiki.archlinux.org/title/Installation_guide#Unmount_the_root_partition) and [ArchWiki Installation Guide#Reboot](https://wiki.archlinux.org/title/Installation_guide#Reboot).

**ENVIRONMENT:** Chroot (root@archiso /)# → Live USB (root@archiso)
**PREREQUISITES:** All configuration completed

## Commands

```bash
# Exit chroot. See [ArchWiki: Chroot](https://wiki.archlinux.org/title/Chroot).
exit

# Unmount all partitions. See [ArchWiki: Mount#Unmounting](https://wiki.archlinux.org/title/Mount#Unmounting).
umount -R /mnt

# Close encrypted volumes (if used). See [ArchWiki: dm-crypt/Device encryption](https://wiki.archlinux.org/title/Dm-crypt/Device_encryption#Closing_the_device).
cryptsetup close cryptswap 2>/dev/null
cryptsetup close cryptroot 2>/dev/null

# Reboot. See [ArchWiki: systemd#Power_management](https://wiki.archlinux.org/title/Systemd#Power_management).
reboot
```

**Remove USB drive** when system shuts down (before it boots again).

**INSTALLATION COMPLETE!**

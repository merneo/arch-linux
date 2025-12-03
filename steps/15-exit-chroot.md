# Step: Exit Chroot and Reboot

**ENVIRONMENT:** Chroot (root@archiso /)# → Live USB (root@archiso)
**PREREQUISITES:** All configuration completed

## Commands

```bash
# Exit chroot
exit

# Unmount all partitions
umount -R /mnt

# Close encrypted volumes (if used)
cryptsetup close cryptswap 2>/dev/null
cryptsetup close cryptroot 2>/dev/null

# Reboot
reboot
```

**Remove USB drive** when system shuts down (before it boots again).

**INSTALLATION COMPLETE!**

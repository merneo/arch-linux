# Module: Enter Chroot Environment

**Purpose:** Change root into the installed system to configure it. This is a critical step in the Arch Linux installation, effectively placing your shell environment inside the newly installed system's filesystem hierarchy. For more details, refer to the [ArchWiki on arch-chroot](https://wiki.archlinux.org/title/Arch-chroot).

**Prerequisites:**
- Base system installed (module `core-installation.md`)
- Fstab generated

**Time:** 1 minute

**ENVIRONMENT:** Live USB (root@archiso) → Chroot (root@archiso /)#

---

## Enter Chroot

```bash
arch-chroot /mnt
```

**Prompt changes to:**
```
[root@archiso /]#
```

**SUCCESS:** You are now inside the new Arch system

**Note:** All following commands run inside chroot until you type `exit`. The `chroot` command changes the apparent root directory for the current running process and its children. It is commonly used for system recovery, building packages, or performing maintenance on a system when not running from its native root. For further reading, see the [ArchWiki on chroot](https://wiki.archlinux.org/title/Chroot).

---

---

## Troubleshooting

For more extensive troubleshooting on chroot, refer to the [ArchWiki on arch-chroot#Troubleshooting](https://wiki.archlinux.org/title/Arch-chroot#Troubleshooting).

### Problem: arch-chroot command not found
**Solution:**
1. Ensure you're booted from Arch Linux Live USB
2. Verify you're in Live USB environment: `hostname` should show `archiso`
3. If using regular `chroot`, use `arch-chroot` instead for proper Arch Linux setup

### Problem: Cannot access /mnt
**Solution:**
1. Verify base system is installed: `ls /mnt/bin /mnt/etc`
2. Check partitions are mounted: `mount | grep /mnt`
3. Ensure fstab was generated: `cat /mnt/etc/fstab`

### Problem: Commands don't work inside chroot
**Solution:**
1. Verify you're actually in chroot (prompt should show `/]#`)
2. Check that base system was installed correctly
3. Try `arch-chroot /mnt /bin/bash` to explicitly use bash

---

**Next:** Use configuration modules:
- `locale.md` - Set locale and timezone
- `user-creation.md` - Create user account
- `grub.md` - Install GRUB bootloader

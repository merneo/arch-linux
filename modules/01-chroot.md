# Module: Enter Chroot Environment

**Purpose:** Change root into the installed system to configure it

**Prerequisites:**
- Base system installed (module `00-core-installation.md`)
- Fstab generated

**Time:** 1 minute

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

**Note:** All following commands run inside chroot until you type `exit`

---

**Next:** Use configuration modules:
- `02-locale.md` - Set locale and timezone
- `03-root-password.md` - Set root password
- `04-user-creation.md` - Create user account
- `05-grub.md` - Install GRUB bootloader

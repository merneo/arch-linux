# Module: Enter Chroot Environment

**Purpose:** Change root into the installed system to configure it. This is a critical step in the Arch Linux installation, effectively placing your shell environment inside the newly installed system's filesystem hierarchy. For more details, refer to the [ArchWiki on arch-chroot](https://wiki.archlinux.org/title/Arch-chroot).

**Prerequisites:**
- Base system installed (module `00-core-installation.md`)
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

**Next:** Use configuration modules:
- `02-locale.md` - Set locale and timezone
- `03-root-password.md` - Set root password
- `04-user-creation.md` - Create user account
- `05-grub.md` - Install GRUB bootloader

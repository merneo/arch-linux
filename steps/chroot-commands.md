# Step: Enter Chroot Environment

**Purpose:** Change root into the installed system to configure it. For more details, refer to the [ArchWiki on arch-chroot](https://wiki.archlinux.org/title/Arch-chroot).

**ENVIRONMENT:** Live USB (root@archiso) → Chroot (root@archiso /)#
**PREREQUISITES:** Base system installed, fstab generated

## Commands

```bash
arch-chroot /mnt
```

**Prompt changes to:** `[root@archiso /]#`

**NEXT:** `locale.md`

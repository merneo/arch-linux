# Step: Create User Account

**Purpose:** Create non-root user account with sudo access. This is a fundamental security practice. For more information, refer to the [ArchWiki on Users and groups](https://wiki.archlinux.org/title/Users_and_groups) and [ArchWiki on Sudo](https://wiki.archlinux.org/title/Sudo).

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment, sudo package installed

## Commands

```bash
# Create user (replace <USERNAME> with your desired username). See [ArchWiki: useradd](https://wiki.archlinux.org/title/Users_and_groups#User_management).
useradd -m -G wheel,video,audio,storage,power,network -s /bin/bash <USERNAME>

# Set user password. See [ArchWiki: passwd](https://wiki.archlinux.org/title/Passwd).
passwd <USERNAME>

# Configure sudo access. See [ArchWiki: Sudo](https://wiki.archlinux.org/title/Sudo).
EDITOR=nano visudo
# Uncomment line: %wheel ALL=(ALL:ALL) ALL
```

## Verification

```bash
id <USERNAME> # Verify user information. See [ArchWiki: id](https://wiki.archlinux.org/title/Users_and_groups#User_information).
# Should show: uid=1000, groups include wheel, video, audio, etc.
```

**NEXT:** `05-grub.md`

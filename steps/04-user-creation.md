# Step: Create User Account

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment, sudo package installed

## Commands

```bash
# Create user (replace <USERNAME> with your desired username)
useradd -m -G wheel,video,audio,storage,input,power,network -s /bin/bash <USERNAME>

# Set user password
passwd <USERNAME>

# Configure sudo access
EDITOR=nano visudo
# Uncomment line: %wheel ALL=(ALL:ALL) ALL
```

## Verification

```bash
id <USERNAME>
# Should show: uid=1000, groups include wheel, video, audio, etc.
```

**NEXT:** `05-grub.md`

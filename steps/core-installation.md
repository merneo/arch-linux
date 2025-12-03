# Step: Core System Installation

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Network connected, partitions mounted at /mnt

## Commands

```bash
# Update mirrorlist (optional, faster mirrors). For details, see [ArchWiki: Mirrors](https://wiki.archlinux.org/title/Mirrors) and [ArchWiki: reflector](https://wiki.archlinux.org/title/Reflector).
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup
reflector --country US,Germany,France --age 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist

# Install base system. This uses `pacstrap` to install the base packages to the new root.
# For more info, refer to [ArchWiki: pacstrap](https://wiki.archlinux.org/title/Pacstrap) and [ArchWiki: Installation guide#Install essential packages](https://wiki.archlinux.org/title/Installation_guide#Install_essential_packages).
pacstrap /mnt \
  base \
  base-devel \
  linux \
  linux-firmware \
  linux-headers \
  btrfs-progs \
  dosfstools \
  e2fsprogs \
  networkmanager \
  network-manager-applet \
  vim \
  nano \
  git \
  sudo \
  man-db \
  man-pages

# Generate fstab. The `genfstab` utility generates an fstab file for the mounted filesystems.
# See [ArchWiki: fstab](https://wiki.archlinux.org/title/Fstab) and [ArchWiki: genfstab](https://wiki.archlinux.org/title/Genfstab).
genfstab -U /mnt >> /mnt/etc/fstab
```

## Verification

```bash
ls -la /mnt/bin /mnt/etc /mnt/usr
cat /mnt/etc/fstab # Verify fstab was generated. See [ArchWiki: fstab](https://wiki.archlinux.org/title/Fstab).
```

**NEXT:** `chroot.md`

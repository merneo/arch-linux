# Step: Core System Installation

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Network connected, partitions mounted at /mnt

## Commands

```bash
# Update mirrorlist (optional, faster mirrors)
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup
reflector --country US,Germany,France --age 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist

# Install base system
pacstrap /mnt base base-devel linux linux-firmware linux-headers btrfs-progs dosfstools e2fsprogs networkmanager network-manager-applet vim nano git sudo man-db man-pages

# Generate fstab
genfstab -U /mnt >> /mnt/etc/fstab
```

## Verification

```bash
ls -la /mnt/bin /mnt/etc /mnt/usr
cat /mnt/etc/fstab
```

**NEXT:** `01-chroot.md`

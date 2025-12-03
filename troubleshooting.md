# Troubleshooting Guide

**Purpose:** Centralized troubleshooting guide for common installation and configuration issues. This guide aggregates troubleshooting information from all modules and phases.

**Quick Links:**
- [Installation Checklist](installation-checklist.md) - Track your progress
- [FAQ](faq.md) - Frequently asked questions
- [Common Mistakes](common-mistakes.md) - Avoid common errors

---

## 🚨 Boot Issues

### GRUB Menu Not Appearing

**Symptoms:**
- System boots directly to Windows (dual boot)
- Black screen on boot
- No boot menu

**Solutions:**
1. **Check EFI partition:**
   ```bash
   ls -la /boot/EFI/GRUB/
   ```
   If files are missing, reinstall GRUB. See [GRUB Module](modules/grub.md).

2. **Verify GRUB installation:**
   ```bash
   arch-chroot /mnt
   grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
   grub-mkconfig -o /boot/grub/grub.cfg
   ```

3. **Check BIOS/UEFI settings:**
   - Ensure UEFI boot is enabled
   - Check boot order (Arch Linux should be first)
   - Disable Secure Boot if enabled

**See:** [GRUB Troubleshooting](modules/grub.md#troubleshooting)

---

### System Won't Boot After Installation

**Symptoms:**
- GRUB appears but system hangs
- Kernel panic
- Boot loop

**Solutions:**
1. **Check root UUID in GRUB:**
   ```bash
   # Boot from Live USB, chroot, check:
   blkid
   cat /etc/default/grub
   ```
   Ensure `GRUB_CMDLINE_LINUX` has correct UUID.

2. **Verify filesystem:**
   ```bash
   # From Live USB:
   mount /dev/sdXY /mnt
   ls -la /mnt/bin /mnt/etc
   ```

3. **Check for LUKS issues:**
   - Verify cryptdevice UUID is correct
   - Ensure LUKS volume can be opened

**See:** [GRUB Troubleshooting](modules/grub.md#troubleshooting) | [LUKS Troubleshooting](modules/luks-encryption.md#troubleshooting)

---

### LUKS Passphrase Not Working

**Symptoms:**
- Passphrase prompt appears but doesn't accept password
- "No key available" error

**Solutions:**
1. **Verify keyboard layout:**
   - GRUB may use different keyboard layout
   - Try typing passphrase carefully

2. **Check LUKS slot:**
   ```bash
   # From Live USB:
   cryptsetup luksDump /dev/sdXY
   ```
   Verify key slots are active.

3. **Test passphrase:**
   ```bash
   cryptsetup open --test-passphrase /dev/sdXY
   ```

**See:** [LUKS Encryption Troubleshooting](modules/luks-encryption.md#troubleshooting)

---

## 💾 Disk & Filesystem Issues

### Cannot Mount Partitions

**Symptoms:**
- `mount: /mnt: wrong fs type` error
- `mount: special device /dev/sdXY does not exist`

**Solutions:**
1. **Check partition exists:**
   ```bash
   lsblk
   fdisk -l
   ```

2. **Verify filesystem type:**
   ```bash
   lsblk -f
   ```
   Ensure filesystem matches what you're trying to mount.

3. **Check if already mounted:**
   ```bash
   mount | grep /mnt
   umount /mnt  # if needed
   ```

**See:** [Mount Partitions Troubleshooting](modules/mount-partitions.md#troubleshooting)

---

### Btrfs Subvolume Issues

**Symptoms:**
- Cannot create subvolumes
- Subvolumes not mounting correctly

**Solutions:**
1. **Verify Btrfs filesystem:**
   ```bash
   btrfs filesystem show
   ```

2. **Check subvolume creation:**
   ```bash
   btrfs subvolume list /mnt
   ```

3. **Verify mount options:**
   ```bash
   mount | grep /mnt
   ```
   Should show `subvol=@` or similar.

**See:** [Btrfs Filesystem Troubleshooting](modules/btrfs-filesystem.md#troubleshooting)

---

### Disk Partitioning Errors

**Symptoms:**
- `cfdisk` fails
- Cannot write partition table
- "No space left on device"

**Solutions:**
1. **Check disk space:**
   ```bash
   df -h
   lsblk
   ```

2. **Verify disk is not in use:**
   ```bash
   lsof | grep /dev/sdX
   umount /dev/sdXY  # if mounted
   ```

3. **Check for existing partitions:**
   ```bash
   fdisk -l /dev/sdX
   ```

**See:** [Disk Partitioning Troubleshooting](modules/disk-partitioning.md#troubleshooting)

---

## 🔧 System Installation Issues

### Pacstrap Fails

**Symptoms:**
- `pacstrap` command fails
- Package download errors
- "Failed to commit transaction"

**Solutions:**
1. **Check network:**
   ```bash
   ping archlinux.org
   ip link
   ```

2. **Update mirror list:**
   ```bash
   reflector --country "Czech Republic" --latest 10 --sort rate --save /etc/pacman.d/mirrorlist
   ```

3. **Check disk space:**
   ```bash
   df -h /mnt
   ```
   Ensure at least 5 GB free.

4. **Try again with verbose output:**
   ```bash
   pacstrap -K /mnt base linux linux-firmware
   ```

**See:** [Core Installation Troubleshooting](modules/core-installation.md#troubleshooting)

---

### Chroot Issues

**Symptoms:**
- `arch-chroot` fails
- "command not found" in chroot
- Cannot access `/mnt`

**Solutions:**
1. **Verify mounts:**
   ```bash
   mount | grep /mnt
   ```
   Ensure `/mnt` is mounted.

2. **Check chroot command:**
   ```bash
   arch-chroot /mnt
   ```
   Not `chroot /mnt` (missing bind mounts).

3. **Verify system installed:**
   ```bash
   ls -la /mnt/bin /mnt/etc
   ```

**See:** [Chroot Troubleshooting](modules/chroot.md#troubleshooting)

---

## 🌐 Network Issues

### No Network Connection

**Symptoms:**
- Cannot ping archlinux.org
- `ip link` shows no connection
- NetworkManager not working

**Solutions:**
1. **Check network interface:**
   ```bash
   ip link
   ```
   Ensure interface is UP.

2. **For Ethernet:**
   ```bash
   dhcpcd  # or
   systemctl start NetworkManager
   ```

3. **For WiFi:**
   ```bash
   iwctl
   station wlan0 scan
   station wlan0 get-networks
   station wlan0 connect <SSID>
   ```

4. **Check NetworkManager:**
   ```bash
   systemctl status NetworkManager
   systemctl start NetworkManager
   ```

**See:** [NetworkManager Troubleshooting](modules/networkmanager.md#troubleshooting) | [WiFi Troubleshooting](modules/wifi.md#troubleshooting)

---

### WiFi Not Working

**Symptoms:**
- WiFi adapter not detected
- Cannot connect to networks
- Connection drops frequently

**Solutions:**
1. **Check WiFi adapter:**
   ```bash
   lspci | grep -i network
   lsusb | grep -i wireless
   ```

2. **Install WiFi drivers:**
   ```bash
   pacman -S networkmanager wireless_tools wpa_supplicant
   ```

3. **Enable NetworkManager:**
   ```bash
   systemctl enable NetworkManager
   systemctl start NetworkManager
   ```

4. **Use iwctl (in Live USB):**
   ```bash
   iwctl
   device list
   station wlan0 scan
   station wlan0 connect <SSID>
   ```

**See:** [WiFi Troubleshooting](modules/wifi.md#troubleshooting)

---

## 🔊 Audio Issues

### No Sound

**Symptoms:**
- No audio output
- Applications can't find audio device
- PipeWire not working

**Solutions:**
1. **Check audio devices:**
   ```bash
   pactl list short sinks
   ```

2. **Check PipeWire services:**
   ```bash
   systemctl --user status pipewire
   systemctl --user start pipewire
   systemctl --user start pipewire-pulse
   ```

3. **Verify audio hardware:**
   ```bash
   lspci | grep -i audio
   lsusb | grep -i audio
   ```

4. **Install audio packages:**
   ```bash
   pacman -S pipewire pipewire-pulse pipewire-alsa pipewire-jack
   ```

**See:** [Audio Troubleshooting](modules/audio.md#troubleshooting)

---

## 🖥️ Display & Graphics Issues

### No Display Output

**Symptoms:**
- Black screen after boot
- No graphics output
- Xorg/Wayland fails to start

**Solutions:**
1. **Check GPU:**
   ```bash
   lspci | grep -i vga
   lspci | grep -i 3d
   ```

2. **Install GPU drivers:**
   - NVIDIA: [NVIDIA Drivers](modules/nvidia-drivers.md)
   - AMD: [AMD Drivers](modules/amd-drivers.md)

3. **Check Xorg/Wayland:**
   ```bash
   journalctl -b | grep -i xorg
   journalctl -b | grep -i wayland
   ```

**See:** [Xorg Troubleshooting](modules/xorg-config.md#troubleshooting) | [Wayland Troubleshooting](modules/wayland-config.md#troubleshooting)

---

### NVIDIA GPU Issues

**Symptoms:**
- `nvidia-smi` fails
- Black screen with NVIDIA
- Poor performance

**Solutions:**
1. **Verify driver installation:**
   ```bash
   pacman -Qs nvidia
   nvidia-smi
   ```

2. **Check kernel modules:**
   ```bash
   lsmod | grep nvidia
   modprobe nvidia
   ```

3. **Regenerate initramfs:**
   ```bash
   mkinitcpio -P
   ```

**See:** [NVIDIA Drivers Troubleshooting](modules/nvidia-drivers.md#troubleshooting)

---

## 👤 User & Permissions Issues

### Cannot Use Sudo

**Symptoms:**
- "user is not in the sudoers file"
- Sudo command not found
- Permission denied errors

**Solutions:**
1. **Check user groups:**
   ```bash
   groups
   id
   ```
   User should be in `wheel` group.

2. **Configure sudo:**
   ```bash
   visudo
   ```
   Uncomment: `%wheel ALL=(ALL:ALL) ALL`

3. **Add user to wheel:**
   ```bash
   usermod -aG wheel <username>
   ```

**See:** [User Creation Troubleshooting](modules/user-creation.md#troubleshooting)

---

## 🔒 Security Issues

### SSH Not Working

**Symptoms:**
- Cannot connect via SSH
- Connection refused
- Permission denied

**Solutions:**
1. **Check SSH service:**
   ```bash
   systemctl status sshd
   systemctl start sshd
   systemctl enable sshd
   ```

2. **Verify SSH configuration:**
   ```bash
   cat /etc/ssh/sshd_config | grep Port
   cat /etc/ssh/sshd_config | grep PermitRootLogin
   ```

3. **Check firewall:**
   ```bash
   ufw status
   ufw allow 1991/tcp
   ```

**See:** [SSH Server Troubleshooting](modules/ssh-server.md#troubleshooting)

---

### UFW Firewall Issues

**Symptoms:**
- Cannot connect to services
- Firewall blocking everything
- UFW not working

**Solutions:**
1. **Check UFW status:**
   ```bash
   ufw status verbose
   ```

2. **Allow necessary ports:**
   ```bash
   ufw allow 1991/tcp  # SSH
   ufw allow out 53/udp  # DNS
   ```

3. **Check UFW rules:**
   ```bash
   ufw status numbered
   ```

**See:** [UFW Firewall Troubleshooting](modules/ufw-firewall.md#troubleshooting)

---

## 💻 Laptop Hardware Issues

### Touchpad Not Working

**Symptoms:**
- Touchpad not responding
- No touchpad detected
- Gestures not working

**Solutions:**
1. **Check touchpad:**
   ```bash
   libinput list-devices | grep -i touchpad
   xinput list
   ```

2. **Install touchpad drivers:**
   ```bash
   pacman -S xf86-input-libinput
   ```

3. **Configure touchpad:**
   See [Touchpad Module](modules/touchpad.md)

**See:** [Touchpad Troubleshooting](modules/touchpad.md#troubleshooting)

---

### Webcam Not Working

**Symptoms:**
- Webcam not detected
- Applications can't access webcam
- No video input

**Solutions:**
1. **Check webcam:**
   ```bash
   lsusb | grep -i camera
   v4l2-ctl --list-devices
   ```

2. **Install webcam packages:**
   ```bash
   pacman -S v4l-utils
   ```

3. **Check permissions:**
   - Ensure user is in `video` group
   - Check application permissions

**See:** [Webcam Troubleshooting](modules/webcam.md#troubleshooting)

---

## 🎯 General Troubleshooting Tips

### Check Logs
```bash
# System logs
journalctl -b  # Current boot
journalctl -xe  # Recent errors

# Service logs
systemctl status <service>
journalctl -u <service>

# Boot logs
dmesg | less
```

### Verify Installation
```bash
# Check installed packages
pacman -Q

# Check services
systemctl list-units --type=service --state=running

# Check mounts
mount | grep -E "^/dev"
```

### Get Help
- [ArchWiki](https://wiki.archlinux.org/) - Comprehensive documentation
- [Arch Linux Forums](https://bbs.archlinux.org/) - Community support
- [Installation Checklist](installation-checklist.md) - Verify steps
- [FAQ](faq.md) - Common questions

---

**Back to:** [Main Index](index.md) | [Repository Root](README.md)

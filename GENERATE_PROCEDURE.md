# Generate Installation Procedure

**Purpose:** Guide to generate a custom installation procedure based on your needs. This document empowers you to build a personalized Arch Linux installation process by selecting and ordering modular components. For a broader understanding of the installation process and its customization, refer to the [ArchWiki Installation Guide](https://wiki.archlinux.org/title/Installation_guide).

---

## Step 1: Assess Your Situation

Answer these questions to select the modules relevant to your installation:

### Disk Status
- [ ] Disk is already partitioned → Skip [`disk-partitioning.md`](modules/disk-partitioning.md) ([ArchWiki: Partitioning](https://wiki.archlinux.org/title/Partitioning))
- [ ] Disk needs partitioning → Include [`disk-partitioning.md`](modules/disk-partitioning.md) ([ArchWiki: Partitioning](https://wiki.archlinux.org/title/Partitioning))

### Encryption
- [ ] Want disk encryption → Include [`luks-encryption.md`](modules/luks-encryption.md) ([ArchWiki: dm-crypt](https://wiki.archlinux.org/title/Dm-crypt))
- [ ] Want automatic unlock → Include [`luks-keyfile-auto-unlock.md`](modules/luks-keyfile-auto-unlock.md) ([ArchWiki: dm-crypt/Keyfiles](https://wiki.archlinux.org/title/Dm-crypt#Keyfiles))
- [ ] No encryption needed → Skip LUKS modules

### Filesystem
- [ ] Want Btrfs with subvolumes → Include [`btrfs-filesystem.md`](modules/btrfs-filesystem.md) ([ArchWiki: Btrfs](https://wiki.archlinux.org/title/Btrfs))
- [ ] Want ext4/xfs → Include [`mount-partitions.md`](modules/mount-partitions.md) ([ArchWiki: File systems](https://wiki.archlinux.org/title/File_systems))
- [ ] Filesystem already created → Skip filesystem modules

### Basic Configuration
- [ ] Need locale/timezone → Include [`locale.md`](modules/locale.md) ([ArchWiki: Locale](https://wiki.archlinux.org/title/Locale), [ArchWiki: Time](https://wiki.archlinux.org/title/Time))
- [ ] Need user account → Include [`user-creation.md`](modules/user-creation.md) ([ArchWiki: Users and groups](https://wiki.archlinux.org/title/Users_and_groups))

### Bootloader
- [ ] Need GRUB → Include [`grub.md`](modules/grub.md) ([ArchWiki: GRUB](https://wiki.archlinux.org/title/GRUB))

### Network
- [ ] Need NetworkManager → Include [`networkmanager.md`](modules/networkmanager.md) ([ArchWiki: NetworkManager](https://wiki.archlinux.org/title/NetworkManager))
- [ ] Need WiFi → Include [`wifi.md`](modules/wifi.md) ([ArchWiki: Wireless network configuration](https://wiki.archlinux.org/title/Wireless_network_configuration))
- [ ] Need Bluetooth → Include [`bluetooth.md`](modules/bluetooth.md) ([ArchWiki: Bluetooth](https://wiki.archlinux.org/title/Bluetooth))

### Audio
- [ ] Need audio → Include [`audio.md`](modules/audio.md) ([ArchWiki: PipeWire](https://wiki.archlinux.org/title/PipeWire))

### Security
- [ ] Need SSH Server → Include [`ssh-server.md`](modules/ssh-server.md) ([ArchWiki: OpenSSH](https://wiki.archlinux.org/title/OpenSSH))
- [ ] Need UFW Firewall → Include [`ufw-firewall.md`](modules/ufw-firewall.md) ([ArchWiki: UFW](https://wiki.archlinux.org/title/Uncomplicated_Firewall))
- [ ] Need Fail2ban → Include [`fail2ban.md`](modules/fail2ban.md) ([ArchWiki: Fail2ban](https://wiki.archlinux.org/title/Fail2ban))

### Hardware (If applicable)
- [ ] Need Touchpad config → Include [`touchpad.md`](modules/touchpad.md) ([ArchWiki: Touchpad](https://wiki.archlinux.org/title/Touchpad))
- [ ] Need Webcam config → Include [`webcam.md`](modules/webcam.md) ([ArchWiki: Webcam setup](https://wiki.archlinux.org/title/Webcam_setup))
- [ ] Need IR Camera config → Include [`ir-camera.md`](modules/ir-camera.md) ([ArchWiki: Howdy](https://wiki.archlinux.org/title/Howdy))
- [ ] Need Fingerprint config → Include [`fingerprint.md`](modules/fingerprint.md) ([ArchWiki: Fprint](https://wiki.archlinux.org/title/Fprint))

### Desktop Environment / Display Server
- [ ] Need Xorg Display Server → Include [`xorg-config.md`](modules/xorg-config.md) ([ArchWiki: Xorg](https://wiki.archlinux.org/title/Xorg))
- [ ] Need Wayland Display Server → Include [`wayland-config.md`](modules/wayland-config.md) ([ArchWiki: Wayland](https://wiki.archlinux.org/title/Wayland))
- [ ] Need GNOME → Include [`gnome.md`](modules/gnome.md) ([ArchWiki: GNOME](https://wiki.archlinux.org/title/GNOME))
- [ ] Need KDE Plasma → Include [`kde-plasma.md`](modules/kde-plasma.md) ([ArchWiki: KDE](https://wiki.archlinux.org/title/KDE))
- [ ] Need XFCE → Include [`xfce.md`](modules/xfce.md) ([ArchWiki: Xfce](https://wiki.archlinux.org/title/Xfce))

### Essential Applications / Backup
- [ ] Need Essential Applications → Include [`essential-applications.md`](modules/essential-applications.md) ([ArchWiki: List of applications](https://wiki.archlinux.org/title/List_of_applications))
- [ ] Need Backup Solution (Timeshift) → Include [`timeshift.md`](modules/timeshift.md) ([ArchWiki: Timeshift](https://wiki.archlinux.org/title/Timeshift))
- [ ] Need w3m Terminal Web Browser → Include [`w3m.md`](modules/w3m.md) ([ArchWiki: w3m](https://wiki.archlinux.org/title/W3m))

### GPU Drivers
- [ ] Need NVIDIA GPU Drivers → Include [`nvidia-drivers.md`](modules/nvidia-drivers.md) ([ArchWiki: NVIDIA](https://wiki.archlinux.org/title/NVIDIA))
- [ ] Need AMD GPU Drivers → Include [`amd-drivers.md`](modules/amd-drivers.md) ([ArchWiki: AMDGPU](https://wiki.archlinux.org/title/AMDGPU))

### Security
- [ ] Need SSH Server → Include `ssh-server.md`
- [ ] Need UFW Firewall → Include `ufw-firewall.md`
- [ ] Need Fail2ban → Include `fail2ban.md`

### Hardware (If applicable)
- [ ] Need Touchpad config → Include `touchpad.md`
- [ ] Need Webcam config → Include `webcam.md`
- [ ] Need IR Camera config → Include `ir-camera.md`
- [ ] Need Fingerprint config → Include `fingerprint.md`

### Desktop Environment / Display Server
- [ ] Need Xorg Display Server → Include `xorg-config.md`
- [ ] Need Wayland Display Server → Include `wayland-config.md`
- [ ] Need GNOME → Include `gnome.md`
- [ ] Need KDE Plasma → Include `kde-plasma.md`
- [ ] Need XFCE → Include `xfce.md`

### Essential Applications / Backup
- [ ] Need Essential Applications → Include `essential-applications.md`
- [ ] Need Backup Solution (Timeshift) → Include `timeshift.md`

### GPU Drivers
- [ ] Need NVIDIA GPU Drivers → Include `nvidia-drivers.md`
- [ ] Need AMD GPU Drivers → Include `amd-drivers.md`

---

## Step 2: Build Your Procedure

Start with these **ALWAYS REQUIRED** modules:

1. `core-installation.md` - Install base system
2. `chroot.md` - Enter chroot

Then add modules based on your answers above, in this order:

### Disk Setup:
- `disk-partitioning.md` - Create partitions (if needed)
- `luks-encryption.md` - Encrypt partitions (if needed)
- `btrfs-filesystem.md` - Create Btrfs (if needed)
- `mount-partitions.md` - Mount partitions (if needed, non-Btrfs)

### Configuration (in chroot):
- `locale.md` - Set locale
- `user-creation.md` - Create user
- `grub.md` - Install GRUB

### Advanced (optional, still in chroot):
- `luks-keyfile-auto-unlock.md` - Auto-unlock (if using LUKS)
- `networkmanager.md` - Enable NetworkManager
- `wifi.md` - WiFi support
- `bluetooth.md` - Bluetooth support
- `audio.md` - Audio server

### Final step (in chroot):
- `exit-chroot.md` - Exit and reboot

### Post-Reboot (after first boot, logged in as user):
- `ssh-server.md` - Configure SSH server
- `ufw-firewall.md` - Configure UFW firewall
- `fail2ban.md` - Configure Fail2ban
- `touchpad.md` - Configure touchpad
- `webcam.md` - Configure webcam
- `ir-camera.md` - Configure IR camera
- `fingerprint.md` - Configure fingerprint reader
- `xorg-config.md` - Xorg Display Server config
- `wayland-config.md` - Wayland Display Server config
- `gnome.md` - Install GNOME
- `kde-plasma.md` - Install KDE Plasma
- `xfce.md` - Install XFCE
- `essential-applications.md` - Install essential applications
- `timeshift.md` - Timeshift for system snapshots
- `w3m.md` - w3m Terminal Web Browser
- `nvidia-drivers.md` - NVIDIA GPU Driver Installation
- `amd-drivers.md` - AMD GPU Driver Installation

---

## Step 3: Example Procedures

### Scenario 1: New PC, Disk Already Partitioned, No Encryption

**Modules needed:**
1. `mount-partitions.md` - Mount existing partitions
2. `core-installation.md` - Install base system
3. `chroot.md` - Enter chroot
4. `locale.md` - Set locale
5. `user-creation.md` - Create user
6. `grub.md` - Install GRUB
7. `networkmanager.md` - Enable NetworkManager
8. `exit-chroot.md` - Exit and reboot
9. **After first boot:**
   - `xorg-config.md` - Xorg (or `wayland-config.md`)
   - `gnome.md` - GNOME (or `kde-plasma.md`, `xfce.md`)
   - `essential-applications.md`
   - `timeshift.md`
   - `w3m.md`

### Scenario 2: New PC, Need Full Setup (LUKS + Btrfs + All Features)

**Modules needed:**
1. `disk-partitioning.md` - Create partitions
2. `luks-encryption.md` - Encrypt partitions
3. `btrfs-filesystem.md` - Create Btrfs
4. `core-installation.md` - Install base system
5. `chroot.md` - Enter chroot
6. `locale.md` - Set locale
7. `user-creation.md` - Create user
8. `grub.md` - Install GRUB (with LUKS parameters)
9. `luks-keyfile-auto-unlock.md` - Auto-unlock (optional)
10. `networkmanager.md` - Enable NetworkManager
11. `wifi.md` - WiFi support
12. `bluetooth.md` - Bluetooth support
13. `audio.md` - Audio server
14. `exit-chroot.md` - Exit and reboot
15. **After first boot:**
    - `ssh-server.md`
    - `ufw-firewall.md`
    - `fail2ban.md`
    - `touchpad.md` (if laptop)
    - `webcam.md` (if laptop)
    - `ir-camera.md` (if laptop)
    - `fingerprint.md` (if laptop)
    - `xorg-config.md` (or `wayland-config.md`)
    - `gnome.md` (or `kde-plasma.md`, `xfce.md`)
    - `essential-applications.md`
    - `timeshift.md`
    - `w3m.md`
    - `nvidia-drivers.md` (if NVIDIA)
    - `amd-drivers.md` (if AMD)

### Scenario 3: Minimal Installation (Just System + User + Network)

**Modules needed:**
1. `core-installation.md` - Install base system (assumes `/mnt` is already mounted)
2. `chroot.md` - Enter chroot
3. `locale.md` - Set locale
4. `user-creation.md` - Create user
5. `grub.md` - Install GRUB
6. `networkmanager.md` - Enable NetworkManager
7. `exit-chroot.md` - Exit and reboot
8. **After first boot:**
   - `w3m.md`

---

## Step 4: Verify Dependencies

Before starting, check each module's prerequisites:

- Most modules require `chroot.md` (you must be in [chroot](https://wiki.archlinux.org/title/Chroot) environment)
- [GRUB](https://wiki.archlinux.org/title/GRUB) requires partitions to be mounted
- [LUKS](https://wiki.archlinux.org/title/Dm-crypt) modules require disk partitioning
- [Btrfs](https://wiki.archlinux.org/title/Btrfs) can work with or without LUKS
- Modules executed "After first boot" require user login and network connectivity.

---

## Step 5: Execute Your Procedure

1. **Read each module** before executing
2. **Follow steps in order** (respecting dependencies)
3. **Verify success** after each module
4. **Customize** commands (usernames, timezones, UUIDs, etc.)

---

## Tips

- **Start simple:** Begin with minimal installation, add features later
- **Read prerequisites:** Each module lists what's needed before it
- **Check MODULE_INDEX.md:** Quick reference for all modules
- **Customize:** Replace placeholders (usernames, timezones, UUIDs, etc.) with your values
- **Verify:** Check success messages after each module

---

**Remember:** This system assumes you're already booted from Arch Linux Live USB. It doesn't format disks or create USB drives - it only installs and configures the system.
# Generate Installation Procedure

**Purpose:** Guide to generate a custom installation procedure based on your needs. This document empowers you to build a personalized Arch Linux installation process by selecting and ordering modular components. For a broader understanding of the installation process and its customization, refer to the [ArchWiki Installation Guide](https://wiki.archlinux.org/title/Installation_guide).

---

## Step 1: Assess Your Situation

Answer these questions to select the modules relevant to your installation:

### Disk Status
- [ ] Disk is already partitioned → Skip [`06-disk-partitioning.md`](modules/06-disk-partitioning.md) ([ArchWiki: Partitioning](https://wiki.archlinux.org/title/Partitioning))
- [ ] Disk needs partitioning → Include [`06-disk-partitioning.md`](modules/06-disk-partitioning.md) ([ArchWiki: Partitioning](https://wiki.archlinux.org/title/Partitioning))

### Encryption
- [ ] Want disk encryption → Include [`07-luks-encryption.md`](modules/07-luks-encryption.md) ([ArchWiki: dm-crypt](https://wiki.archlinux.org/title/Dm-crypt))
- [ ] Want automatic unlock → Include [`10-luks-keyfile-auto-unlock.md`](modules/10-luks-keyfile-auto-unlock.md) ([ArchWiki: dm-crypt/Keyfiles](https://wiki.archlinux.org/title/Dm-crypt#Keyfiles))
- [ ] No encryption needed → Skip LUKS modules

### Filesystem
- [ ] Want Btrfs with subvolumes → Include [`08-btrfs-filesystem.md`](modules/08-btrfs-filesystem.md) ([ArchWiki: Btrfs](https://wiki.archlinux.org/title/Btrfs))
- [ ] Want ext4/xfs → Include [`09-mount-partitions.md`](modules/09-mount-partitions.md) ([ArchWiki: File systems](https://wiki.archlinux.org/title/File_systems))
- [ ] Filesystem already created → Skip filesystem modules

### Basic Configuration
- [ ] Need locale/timezone → Include [`02-locale.md`](modules/02-locale.md) ([ArchWiki: Locale](https://wiki.archlinux.org/title/Locale), [ArchWiki: Time](https://wiki.archlinux.org/title/Time))
- [ ] Need user account → Include [`04-user-creation.md`](modules/04-user-creation.md) ([ArchWiki: Users and groups](https://wiki.archlinux.org/title/Users_and_groups))

### Bootloader
- [ ] Need GRUB → Include [`05-grub.md`](modules/05-grub.md) ([ArchWiki: GRUB](https://wiki.archlinux.org/title/GRUB))

### Network
- [ ] Need NetworkManager → Include [`11-networkmanager.md`](modules/11-networkmanager.md) ([ArchWiki: NetworkManager](https://wiki.archlinux.org/title/NetworkManager))
- [ ] Need WiFi → Include [`12-wifi.md`](modules/12-wifi.md) ([ArchWiki: Wireless network configuration](https://wiki.archlinux.org/title/Wireless_network_configuration))
- [ ] Need Bluetooth → Include [`13-bluetooth.md`](modules/13-bluetooth.md) ([ArchWiki: Bluetooth](https://wiki.archlinux.org/title/Bluetooth))

### Audio
- [ ] Need audio → Include [`14-audio.md`](modules/14-audio.md) ([ArchWiki: PipeWire](https://wiki.archlinux.org/title/PipeWire))

### Security
- [ ] Need SSH Server → Include [`21-ssh-server.md`](modules/21-ssh-server.md) ([ArchWiki: OpenSSH](https://wiki.archlinux.org/title/OpenSSH))
- [ ] Need UFW Firewall → Include [`22-ufw-firewall.md`](modules/22-ufw-firewall.md) ([ArchWiki: UFW](https://wiki.archlinux.org/title/Uncomplicated_Firewall))
- [ ] Need Fail2ban → Include [`23-fail2ban.md`](modules/23-fail2ban.md) ([ArchWiki: Fail2ban](https://wiki.archlinux.org/title/Fail2ban))

### Laptop Hardware (If applicable)
- [ ] Need Touchpad config → Include [`17-laptop-touchpad.md`](modules/17-laptop-touchpad.md) ([ArchWiki: Touchpad](https://wiki.archlinux.org/title/Touchpad))
- [ ] Need Webcam config → Include [`18-laptop-webcam.md`](modules/18-laptop-webcam.md) ([ArchWiki: Webcam setup](https://wiki.archlinux.org/title/Webcam_setup))
- [ ] Need IR Camera config → Include [`19-laptop-ir-camera.md`](modules/19-laptop-ir-camera.md) ([ArchWiki: Howdy](https://wiki.archlinux.org/title/Howdy))
- [ ] Need Fingerprint config → Include [`20-laptop-fingerprint.md`](modules/20-laptop-fingerprint.md) ([ArchWiki: Fprint](https://wiki.archlinux.org/title/Fprint))

### Desktop Environment / Display Server
- [ ] Need Xorg Display Server → Include [`27-xorg-config.md`](modules/27-xorg-config.md) ([ArchWiki: Xorg](https://wiki.archlinux.org/title/Xorg))
- [ ] Need Wayland Display Server → Include [`28-wayland-config.md`](modules/28-wayland-config.md) ([ArchWiki: Wayland](https://wiki.archlinux.org/title/Wayland))
- [ ] Need GNOME → Include [`24-gnome.md`](modules/24-gnome.md) ([ArchWiki: GNOME](https://wiki.archlinux.org/title/GNOME))
- [ ] Need KDE Plasma → Include [`25-kde-plasma.md`](modules/25-kde-plasma.md) ([ArchWiki: KDE](https://wiki.archlinux.org/title/KDE))
- [ ] Need XFCE → Include [`26-xfce.md`](modules/26-xfce.md) ([ArchWiki: Xfce](https://wiki.archlinux.org/title/Xfce))

### Essential Applications / Backup
- [ ] Need Essential Applications → Include [`29-essential-applications.md`](modules/29-essential-applications.md) ([ArchWiki: List of applications](https://wiki.archlinux.org/title/List_of_applications))
- [ ] Need Backup Solution (Timeshift) → Include [`30-timeshift.md`](modules/30-timeshift.md) ([ArchWiki: Timeshift](https://wiki.archlinux.org/title/Timeshift))

### GPU Drivers
- [ ] Need NVIDIA GPU Drivers → Include [`31-nvidia-drivers.md`](modules/31-nvidia-drivers.md) ([ArchWiki: NVIDIA](https://wiki.archlinux.org/title/NVIDIA))
- [ ] Need AMD GPU Drivers → Include [`32-amd-drivers.md`](modules/32-amd-drivers.md) ([ArchWiki: AMDGPU](https://wiki.archlinux.org/title/AMDGPU))

### Security
- [ ] Need SSH Server → Include `21-ssh-server.md`
- [ ] Need UFW Firewall → Include `22-ufw-firewall.md`
- [ ] Need Fail2ban → Include `23-fail2ban.md`

### Laptop Hardware (If applicable)
- [ ] Need Touchpad config → Include `17-laptop-touchpad.md`
- [ ] Need Webcam config → Include `18-laptop-webcam.md`
- [ ] Need IR Camera config → Include `19-laptop-ir-camera.md`
- [ ] Need Fingerprint config → Include `20-laptop-fingerprint.md`

### Desktop Environment / Display Server
- [ ] Need Xorg Display Server → Include `27-xorg-config.md`
- [ ] Need Wayland Display Server → Include `28-wayland-config.md`
- [ ] Need GNOME → Include `24-gnome.md`
- [ ] Need KDE Plasma → Include `25-kde-plasma.md`
- [ ] Need XFCE → Include `26-xfce.md`

### Essential Applications / Backup
- [ ] Need Essential Applications → Include `29-essential-applications.md`
- [ ] Need Backup Solution (Timeshift) → Include `30-timeshift.md`

### GPU Drivers
- [ ] Need NVIDIA GPU Drivers → Include `31-nvidia-drivers.md`
- [ ] Need AMD GPU Drivers → Include `32-amd-drivers.md`

---

## Step 2: Build Your Procedure

Start with these **ALWAYS REQUIRED** modules:

1. `00-core-installation.md` - Install base system
2. `01-chroot.md` - Enter chroot

Then add modules based on your answers above, in this order:

### Disk Setup:
- `06-disk-partitioning.md` - Create partitions (if needed)
- `07-luks-encryption.md` - Encrypt partitions (if needed)
- `08-btrfs-filesystem.md` - Create Btrfs (if needed)
- `09-mount-partitions.md` - Mount partitions (if needed, non-Btrfs)

### Configuration (in chroot):
- `02-locale.md` - Set locale
- `04-user-creation.md` - Create user
- `05-grub.md` - Install GRUB

### Advanced (optional, still in chroot):
- `10-luks-keyfile-auto-unlock.md` - Auto-unlock (if using LUKS)
- `11-networkmanager.md` - Enable NetworkManager
- `12-wifi.md` - WiFi support
- `13-bluetooth.md` - Bluetooth support
- `14-audio.md` - Audio server

### Final step (in chroot):
- `15-exit-chroot.md` - Exit and reboot

### Post-Reboot (after first boot, logged in as user):
- `21-ssh-server.md` - Configure SSH server
- `22-ufw-firewall.md` - Configure UFW firewall
- `23-fail2ban.md` - Configure Fail2ban
- `17-laptop-touchpad.md` - Configure touchpad
- `18-laptop-webcam.md` - Configure webcam
- `19-laptop-ir-camera.md` - Configure IR camera
- `20-laptop-fingerprint.md` - Configure fingerprint reader
- `27-xorg-config.md` - Xorg Display Server config
- `28-wayland-config.md` - Wayland Display Server config
- `24-gnome.md` - Install GNOME
- `25-kde-plasma.md` - Install KDE Plasma
- `26-xfce.md` - Install XFCE
- `29-essential-applications.md` - Install essential applications
- `30-timeshift.md` - Timeshift for system snapshots
- `31-nvidia-drivers.md` - NVIDIA GPU Driver Installation
- `32-amd-drivers.md` - AMD GPU Driver Installation

---

## Step 3: Example Procedures

### Scenario 1: New PC, Disk Already Partitioned, No Encryption

**Modules needed:**
1. `09-mount-partitions.md` - Mount existing partitions
2. `00-core-installation.md` - Install base system
3. `01-chroot.md` - Enter chroot
4. `02-locale.md` - Set locale
5. `04-user-creation.md` - Create user
6. `05-grub.md` - Install GRUB
7. `11-networkmanager.md` - Enable NetworkManager
8. `15-exit-chroot.md` - Exit and reboot
9. **After first boot:**
   - `27-xorg-config.md` - Xorg (or `28-wayland-config.md`)
   - `24-gnome.md` - GNOME (or `25-kde-plasma.md`, `26-xfce.md`)
   - `29-essential-applications.md`
   - `30-timeshift.md`

### Scenario 2: New PC, Need Full Setup (LUKS + Btrfs + All Features)

**Modules needed:**
1. `06-disk-partitioning.md` - Create partitions
2. `07-luks-encryption.md` - Encrypt partitions
3. `08-btrfs-filesystem.md` - Create Btrfs
4. `00-core-installation.md` - Install base system
5. `01-chroot.md` - Enter chroot
6. `02-locale.md` - Set locale
7. `04-user-creation.md` - Create user
8. `05-grub.md` - Install GRUB (with LUKS parameters)
9. `10-luks-keyfile-auto-unlock.md` - Auto-unlock (optional)
10. `11-networkmanager.md` - Enable NetworkManager
11. `12-wifi.md` - WiFi support
12. `13-bluetooth.md` - Bluetooth support
13. `14-audio.md` - Audio server
14. `15-exit-chroot.md` - Exit and reboot
15. **After first boot:**
    - `21-ssh-server.md`
    - `22-ufw-firewall.md`
    - `23-fail2ban.md`
    - `17-laptop-touchpad.md` (if laptop)
    - `18-laptop-webcam.md` (if laptop)
    - `19-laptop-ir-camera.md` (if laptop)
    - `20-laptop-fingerprint.md` (if laptop)
    - `27-xorg-config.md` (or `28-wayland-config.md`)
    - `24-gnome.md` (or `25-kde-plasma.md`, `26-xfce.md`)
    - `29-essential-applications.md`
    - `30-timeshift.md`
    - `31-nvidia-drivers.md` (if NVIDIA)
    - `32-amd-drivers.md` (if AMD)

### Scenario 3: Minimal Installation (Just System + User + Network)

**Modules needed:**
1. `00-core-installation.md` - Install base system (assumes `/mnt` is already mounted)
2. `01-chroot.md` - Enter chroot
3. `02-locale.md` - Set locale
4. `04-user-creation.md` - Create user
5. `05-grub.md` - Install GRUB
6. `11-networkmanager.md` - Enable NetworkManager
7. `15-exit-chroot.md` - Exit and reboot

---

## Step 4: Verify Dependencies

Before starting, check each module's prerequisites:

- Most modules require `01-chroot.md` (you must be in [chroot](https://wiki.archlinux.org/title/Chroot) environment)
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
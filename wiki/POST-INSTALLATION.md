# Post-Installation Configuration

**Purpose:** Configure system after core installation

**Prerequisites:**
- Core installation completed (see [Core Installation](CORE-INSTALLATION.md))
- Inside chroot environment
- `git` installed (included in core installation)

---

## Menu: Post-Installation Configuration

Choose what you need:

- **[Locale & Timezone](#locale-timezone)** - Set locale and timezone
- **[User Creation](#user-creation)** - Create user account with sudo
- **[GRUB Bootloader](#grub-bootloader)** - Install GRUB (required)
- **[LUKS Auto-Unlock](#luks-auto-unlock)** - Automatic LUKS decryption
- **[NetworkManager](#networkmanager)** - Enable NetworkManager
- **[WiFi Support](#wifi-support)** - WiFi configuration
- **[Bluetooth](#bluetooth)** - Bluetooth support
- **[Audio Server](#audio-server)** - PipeWire audio
- **Post-Chroot Configuration (After First Boot)**
  - **[AUR Helper (e.g., yay)](#aur-helper-eg-yay)** - Install AUR helper
  - **[Laptop Touchpad](#laptop-touchpad)** - Touchpad configuration (laptops only)
  - **[Laptop Webcam](#laptop-webcam)** - Webcam configuration (laptops only)
  - **[Laptop IR Camera](#laptop-ir-camera)** - IR camera for face recognition (laptops only)
  - **[Laptop Fingerprint](#laptop-fingerprint)** - Fingerprint reader (laptops only)
  - **[SSH Server](#ssh-server)** - SSH server (port 1991, root disabled)
  - **[UFW Firewall](#ufw-firewall)** - UFW firewall configuration
  - **[Fail2ban](#fail2ban)** - Fail2ban SSH protection
- **GPU Drivers**
  - **[NVIDIA GPU Driver Installation](#nvidia-gpu-driver-installation)** - Install NVIDIA proprietary drivers
  - **[AMD GPU Driver Installation](#amd-gpu-driver-installation)** - Install AMD open-source drivers
- **Backup Solutions**
  - **[Timeshift for System Snapshots](#timeshift-for-system-snapshots)** - System backup and restore
- **Terminal Tools**
  - **[w3m Terminal Web Browser](#w3m-terminal-web-browser)** - Text-based web browsing
- **Desktop Environments**
  - **[GNOME Desktop Environment](#gnome-desktop-environment)** - Install GNOME
  - **[KDE Plasma Desktop Environment](#kde-plasma-desktop-environment)** - Install KDE Plasma
  - **[XFCE Desktop Environment](#xfce-desktop-environment)** - Install XFCE
- **[Exit & Reboot](#exit-reboot)** - Final step

---

## Locale & Timezone

**Purpose:** Configure system locale and timezone. For more detailed information, refer to the [ArchWiki on Locale](https://wiki.archlinux.org/title/Locale) and [ArchWiki on Time](https://wiki.archlinux.org/title/Time).

**Prerequisites:**
- Inside chroot environment

**Time:** 2-3 minutes

### Step 1: Set Timezone

```bash
# Replace with your timezone (examples: Europe/Prague, America/New_York, Asia/Tokyo)
ln -sf /usr/share/zoneinfo/Europe/Prague /etc/localtime

# Generate /etc/adjtime
hwclock --systohc
```

**Verify timezone:**
```bash
timedatectl status
```

### Step 2: Set Locale

```bash
# Uncomment your locale in locale.gen
# For English (US):
sed -i 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen

# For other locales, edit manually:
# nano /etc/locale.gen
# Uncomment desired locale (e.g., cs_CZ.UTF-8, de_DE.UTF-8)

# Generate locales
locale-gen
```

### Step 3: Set System Locale

```bash
# Set default locale
echo "LANG=en_US.UTF-8" > /etc/locale.conf

# Or for other locales:
# echo "LANG=cs_CZ.UTF-8" > /etc/locale.conf
```

**Verify locale:**
```bash
cat /etc/locale.conf
```

**SUCCESS:** Locale and timezone configured

**Next:** Continue with other configuration steps

---

## User Creation

**Purpose:** Create non-root user account with sudo access. For more detailed information on user management, refer to the [ArchWiki on Users and groups](https://wiki.archlinux.org/title/Users_and_groups) and [ArchWiki on Sudo](https://wiki.archlinux.org/title/Sudo).

**Prerequisites:**
- Inside chroot environment
- Sudo package installed (included in core installation)

**Time:** 2-3 minutes

### Step 1: Create User

```bash
# Replace "username" with your desired username
useradd -m -G wheel,video,audio,storage,power,network -s /bin/bash username

# Explanation:
# -m: Create home directory (/home/username)
# -G: Add to supplementary groups:
#     wheel: sudo access
#     video: GPU access
#     audio: sound access
#     storage: removable media access
#     power: power management
#     network: network configuration
# -s /bin/bash: Default shell (or use /bin/zsh)
```

### Step 2: Set User Password

```bash
passwd username
```

**Enter password** for the user.

### Step 3: Configure Sudo Access

```bash
EDITOR=nano visudo
```

**Find line:**
```bash
# %wheel ALL=(ALL:ALL) ALL
```

**Uncomment (remove #):**
```bash
%wheel ALL=(ALL:ALL) ALL
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

### Step 4: Verify User

```bash
id username

# Should show:
# uid=1000(username) gid=1000(username) groups=1000(username),wheel,video,audio,storage,input,power,network
```

**SUCCESS:** User created with sudo access

**Next:** Continue with other configuration steps

---

## GRUB Bootloader

**Purpose:** Install and configure GRUB bootloader. For comprehensive details, refer to the [ArchWiki on GRUB](https://wiki.archlinux.org/title/GRUB) and [ArchWiki on Unified Extensible Firmware Interface (UEFI)](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface).

**Prerequisites:**
- Inside chroot environment
- EFI partition mounted at `/boot`
- Root partition UUID identified

**Time:** 10-15 minutes

**Note:** This step is **REQUIRED** - system won't boot without GRUB.

### Step 1: Install GRUB

```bash
pacman -S grub efibootmgr os-prober

# Install GRUB to EFI partition
grub-install --target=x86_64-efi \
  --efi-directory=/boot \
  --bootloader-id=GRUB \
  --recheck
```

### Step 2: Get Root Partition UUID

```bash
# Display root partition UUID
blkid /dev/sdX2

# Or filter only UUID value
blkid -s UUID -o value /dev/sdX2
```

**Copy the UUID** (format: `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX`)

### Step 3: Configure GRUB

```bash
nano /etc/default/grub
```

**For standard installation (no encryption):**
```bash
GRUB_CMDLINE_LINUX="root=UUID=<YOUR_UUID>"
GRUB_DISABLE_OS_PROBER=false
```

**For LUKS encryption:**
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<YOUR_UUID>:cryptroot root=/dev/mapper/cryptroot"
GRUB_DISABLE_OS_PROBER=false
```

**For Btrfs subvolume:**
```bash
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<YOUR_UUID>:cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@"
GRUB_DISABLE_OS_PROBER=false
```

**Replace `<YOUR_UUID>` with actual UUID from Step 2**

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

### Step 4: Generate GRUB Configuration

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

**Expected output:**
```
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-linux
Found initrd image: /boot/initramfs-linux.img
done
```

**SUCCESS:** GRUB bootloader installed and configured

**Next:** Continue with other configuration steps or [Exit & Reboot](#exit-reboot)

---

## LUKS Auto-Unlock

**Purpose:** Configure automatic LUKS decryption on boot using keyfile. For further reading, consult the [ArchWiki on dm-crypt/Encrypting an entire system](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system) and [ArchWiki on mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio).

**Prerequisites:**
- Inside chroot environment
- LUKS encryption configured (see [Pre-Installation: LUKS Encryption](PRE-INSTALLATION.md#luks-encryption))
- GRUB configured (see [GRUB Bootloader](#grub-bootloader) above)

**Time:** 10-15 minutes

### Step 1: Create Keyfile Directory

```bash
mkdir -p /etc/cryptsetup.d
chmod 700 /etc/cryptsetup.d
```

### Step 2: Generate LUKS Keyfile

```bash
# Generate 512-byte random keyfile
dd if=/dev/urandom of=/etc/cryptsetup.d/root.key bs=512 count=1

# Set strict permissions
chmod 600 /etc/cryptsetup.d/root.key
```

### Step 3: Add Keyfile to LUKS Key Slot

```bash
# Replace /dev/sdX2 with your root partition
cryptsetup luksAddKey /dev/sdX2 /etc/cryptsetup.d/root.key
```

**Enter existing LUKS passphrase** when prompted.

**Verify keyfile was added:**
```bash
cryptsetup luksDump /dev/sdX2 | grep "Key Slot"

# Should show:
# Key Slot 0: ENABLED
# Key Slot 1: ENABLED
```

### Step 4: Add Keyfile to Swap Partition (if encrypted)

```bash
# Replace /dev/sdX3 with your swap partition
cryptsetup luksAddKey /dev/sdX3 /etc/cryptsetup.d/root.key

# Enter existing passphrase
```

### Step 5: Configure Initramfs to Include Keyfile

```bash
nano /etc/mkinitcpio.conf
```

**Find line:**
```bash
FILES=()
```

**Change to:**
```bash
FILES=(/etc/cryptsetup.d/root.key)
```

**Find HOOKS line:**
```bash
HOOKS=(base udev autodetect modconf block filesystems keyboard fsck)
```

**Add `encrypt` hook (MUST be after `block`, before `filesystems`):**
```bash
HOOKS=(base udev autodetect modconf block encrypt filesystems keyboard fsck)
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

### Step 6: Update GRUB for Keyfile

```bash
nano /etc/default/grub
```

**Find line starting with `GRUB_CMDLINE_LINUX=`**

**Add `cryptkey` parameter:**
```bash
GRUB_CMDLINE_LINUX="cryptkey=rootfs:/etc/cryptsetup.d/root.key cryptdevice=UUID=<YOUR_UUID>:cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@"
```

**Replace `<YOUR_UUID>` with your root partition UUID**

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

### Step 7: Configure Encrypted Swap Auto-Unlock

```bash
# Create /etc/crypttab for swap
cat > /etc/crypttab << EOF
# <name>       <device>                             <keyfile>                          <options>
cryptswap      /dev/sdX3                            /etc/cryptsetup.d/root.key         luks
EOF
```

**Replace `/dev/sdX3` with your swap partition**

**Update fstab for encrypted swap:**
```bash
nano /etc/fstab
```

**Find swap line and change to:**
```
/dev/mapper/cryptswap  none  swap  defaults  0  0
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

### Step 8: Rebuild Initramfs and GRUB

```bash
# Regenerate initramfs (includes keyfile now)
mkinitcpio -P

# Regenerate GRUB config
grub-mkconfig -o /boot/grub/grub.cfg
```

### Step 9: Verify Keyfile is in Initramfs

```bash
# List contents of initramfs
lsinitcpio /boot/initramfs-linux.img | grep root.key

# Should show:
# etc/cryptsetup.d/root.key
```

**SUCCESS:** Automatic LUKS decryption configured

**Note:** System will now boot without asking for LUKS passphrase (keyfile is in `/boot` partition)

**Next:** Continue with other configuration steps

---

## NetworkManager

**Purpose:** Enable NetworkManager for network connectivity. For more information, refer to the [ArchWiki on NetworkManager](https://wiki.archlinux.org/title/NetworkManager) and its command-line interface [nmcli](https://wiki.archlinux.org/title/NetworkManager#nmcli).

**Prerequisites:**
- Inside chroot environment
- NetworkManager installed (included in core installation)

**Time:** 1 minute

### Enable NetworkManager Service

```bash
systemctl enable NetworkManager
```

**Output:**
```
Created symlink /etc/systemd/system/multi-user.target.wants/NetworkManager.service → /usr/lib/systemd/system/NetworkManager.service
```

**SUCCESS:** NetworkManager will start automatically on boot

**After reboot:**
- NetworkManager will automatically connect to wired networks
- Use `nmcli` or `nmtui` to configure WiFi
- Or use NetworkManager GUI applet

**Next:** Continue with other configuration steps

---

## WiFi Support

**Purpose:** Install and configure WiFi support. For a deeper understanding of wireless configuration, consult the [ArchWiki on Wireless network configuration](https://wiki.archlinux.org/title/Wireless_network_configuration) and [wpa_supplicant](https://wiki.archlinux.org/title/WPA_supplicant).

**Prerequisites:**
- Inside chroot environment
- NetworkManager enabled (see [NetworkManager](#networkmanager) above)

**Time:** 2-3 minutes

### Step 1: Install WiFi Packages

```bash
pacman -S wireless_tools wpa_supplicant dialog
```

**Package breakdown:**
- `wireless_tools` - WiFi utilities
- `wpa_supplicant` - WPA/WPA2 authentication
- `dialog` - TUI dialogs (for WiFi setup)

### Step 2: Verify WiFi Support

**After reboot, use NetworkManager to connect:**

**Command line:**
```bash
nmcli device wifi list
nmcli device wifi connect "SSID" password "password"
```

**Text UI:**
```bash
nmtui
```

**GUI:**
- NetworkManager applet (if desktop environment installed)

**SUCCESS:** WiFi support installed

**Note:** WiFi configuration happens after first boot using NetworkManager

**Next:** Continue with other configuration steps

---

## Bluetooth

**Purpose:** Install and enable Bluetooth support. For detailed configuration steps and troubleshooting, refer to the [ArchWiki on Bluetooth](https://wiki.archlinux.org/title/Bluetooth) and the [bluetoothctl](https://wiki.archlinux.org/title/Bluetooth#bluetoothctl) utility.

**Prerequisites:**
- Inside chroot environment

**Time:** 2-3 minutes

### Step 1: Install Bluetooth Packages

```bash
pacman -S bluez bluez-utils
```

**Package breakdown:**
- `bluez` - Bluetooth protocol stack
- `bluez-utils` - bluetoothctl and other tools

### Step 2: Enable Bluetooth Service

```bash
systemctl enable bluetooth
```

**Output:**
```
Created symlink /etc/systemd/system/dbus-org.bluez.service → /usr/lib/systemd/system/bluetooth.service
```

**SUCCESS:** Bluetooth will start automatically on boot

**After reboot:**
- Use `bluetoothctl` to pair devices
- Or use Bluetooth GUI (if desktop environment installed)

**Example usage:**
```bash
bluetoothctl
power on
scan on
pair <device-address>
connect <device-address>
```

**Next:** Continue with other configuration steps

---

## Audio Server

**Purpose:** Install and configure audio server (PipeWire). For comprehensive information and advanced configurations, consult the [ArchWiki on PipeWire](https://wiki.archlinux.org/title/PipeWire).

**Prerequisites:**
- Inside chroot environment

**Time:** 3-5 minutes

### Step 1: Install PipeWire Audio Stack

```bash
pacman -S \
  pipewire \
  pipewire-alsa \
  pipewire-pulse \
  pipewire-jack \
  wireplumber \
  pavucontrol
```

**Package breakdown:**
- `pipewire` - Modern audio server
- `pipewire-alsa` - ALSA compatibility
- `pipewire-pulse` - PulseAudio compatibility
- `pipewire-jack` - JACK compatibility
- `wireplumber` - Session manager
- `pavucontrol` - Audio control GUI

### Step 2: Enable PipeWire Services

```bash
systemctl --user enable pipewire pipewire-pulse wireplumber
```

**Note:** User services are enabled per-user, not system-wide

**SUCCESS:** Audio server installed

**After reboot and login:**
- Audio should work automatically
- Use `pavucontrol` to control audio settings
- PipeWire provides compatibility with ALSA, PulseAudio, and JACK applications

**Next:** Continue with other configuration steps or [Exit & Reboot](#exit-reboot)

---

# Post-Chroot Configuration

## AUR Helper (e.g., yay)

**Purpose:** Install an AUR helper to easily install packages from the Arch User Repository (AUR). The AUR is a community-driven repository for Arch Linux users. For details on the AUR, see the [ArchWiki on the Arch User Repository](https://wiki.archlinux.org/title/Arch_User_Repository). For more information on `yay`, refer to its [GitHub repository](https://github.com/Jguer/yay) or the [ArchWiki on AUR helpers](https://wiki.archlinux.org/title/AUR_helpers#yay).

**Prerequisites:**
- After first boot (not in chroot)
- Git installed (included in core installation)
- User account created with sudo access (see [User Creation](#user-creation))

**Time:** 5-10 minutes

**Note:** This step is only required if you plan to install packages from the AUR, which some laptop-specific configurations may require. The AUR contains user-maintained packages not found in the official Arch repositories.

### Step 1: Install Base-devel and Git

```bash
sudo pacman -S --needed base-devel git
```

### Step 2: Clone yay Repository

```bash
git clone https://aur.archlinux.org/yay.git
```

### Step 3: Build and Install yay

```bash
cd yay
makepkg -si
```

You will be prompted to confirm the installation and any dependencies. Review the package changes carefully.

### Step 4: Clean Up

```bash
cd ..
rm -rf yay
```

**SUCCESS:** yay AUR helper installed.

**Next:** Continue with other configuration steps

---

## Laptop Touchpad

**Purpose:** Configure touchpad/trackpad for laptops. For detailed configuration and troubleshooting, refer to the [ArchWiki on Touchpad Synaptics](https://wiki.archlinux.org/title/Touchpad_Synaptics) and [libinput](https://wiki.archlinux.org/title/Libinput).

**Prerequisites:**
- After first boot (not in chroot)
- Laptop hardware (not desktop)
- Window manager or desktop environment installed

**Time:** 5-10 minutes

**Note:** This is for **LAPTOPS only**. Desktop computers use external mice.

### Step 1: Verify Touchpad Detection

```bash
libinput list-devices | grep -i touchpad
```

### Step 2: Install Required Packages

```bash
pacman -S libinput xf86-input-libinput
```

### Step 3: Configure Touchpad

**For Hyprland (Wayland):**

Edit `~/.config/hypr/hyprland.conf`:
```ini
input {
    touchpad {
        natural_scroll = false
        tap-to-click = true
        disable_while_typing = true
        clickfinger_behavior = true
    }
}
```

**For X11:**

Create `/etc/X11/xorg.conf.d/40-libinput.conf`:
```
Section "InputClass"
    Identifier "libinput touchpad catchall"
    MatchIsTouchpad "on"
    Driver "libinput"
    Option "Tapping" "on"
    Option "DisableWhileTyping" "on"
EndSection
```

**SUCCESS:** Touchpad configured and working

**Official Resources:**
- [ArchWiki: Touchpad Synaptics](https://wiki.archlinux.org/title/Touchpad_Synaptics)
- [ArchWiki: libinput](https://wiki.archlinux.org/title/Libinput)

**Next:** Continue with other configuration steps or [Exit & Reboot](#exit-reboot)

---

## Laptop Webcam

**Purpose:** Configure built-in webcam for laptops. For comprehensive setup information, consult the [ArchWiki on Webcam setup](https://wiki.archlinux.org/title/Webcam_setup).

**Prerequisites:**
- After first boot (not in chroot)
- Laptop hardware (not desktop)
- Webcam hardware present

**Time:** 2-5 minutes

**Note:** This is for **LAPTOPS only**. Desktop computers typically don't have built-in webcams.

### Step 1: Verify Webcam Detection

```bash
lsusb | grep -i camera
ls -la /dev/video*
```

### Step 2: Install Required Packages

```bash
pacman -S v4l-utils
```

### Step 3: Test Webcam

```bash
v4l2-ctl --list-devices
```

**SUCCESS:** Webcam detected and working

**Official Resources:**
- [ArchWiki: Webcam Setup](https://wiki.archlinux.org/title/Webcam_setup)

**Next:** Continue with other configuration steps or [Exit & Reboot](#exit-reboot)

---

## Laptop IR Camera

**Purpose:** Configure IR camera for face recognition (Howdy). For detailed information on this biometric authentication system, refer to the [Howdy GitHub repository](https://github.com/boltgolt/howdy) or the [ArchWiki on Howdy](https://wiki.archlinux.org/title/Howdy).

**Prerequisites:**
- After first boot (not in chroot)
- Laptop hardware (not desktop)
- IR camera hardware present (business laptops)
- Webcam configured (module `webcam.md`)
- AUR Helper installed (see [AUR Helper (e.g., yay)](#aur-helper-eg-yay))

**Time:** 15-30 minutes

**Note:** This is for **LAPTOPS only**. Desktop computers typically don't have built-in IR cameras.

### Step 1: Verify IR Camera Detection

```bash
lsusb | grep -i "ir\|camera"
ls -la /dev/video*
```

### Step 2: Install Howdy

```bash
yay -S howdy-bin
```

### Step 3: Configure Howdy

```bash
sudo nano /lib/security/howdy/config.ini
# Set device_path = /dev/video2 (or your IR camera device)
```

### Step 4: Enroll Face

```bash
sudo howdy add
```

**SUCCESS:** IR camera configured and face recognition working

**Official Resources:**
- [Howdy GitHub](https://github.com/boltgolt/howdy)
- [ArchWiki: Howdy](https://wiki.archlinux.org/title/Howdy)

**Next:** Continue with other configuration steps or [Exit & Reboot](#exit-reboot)

---

## Laptop Fingerprint

**Purpose:** Configure fingerprint reader for laptops. For an in-depth guide, consult the [ArchWiki on Fprint](https://wiki.archlinux.org/title/Fprint) and the [python-validity GitHub repository](https://github.com/uunicorn/python-validity) for specific hardware support.

**Prerequisites:**
- After first boot (not in chroot)
- Laptop hardware (not desktop)
- Fingerprint reader hardware present
- AUR Helper installed (see [AUR Helper (e.g., yay)](#aur-helper-eg-yay))
- **CRITICAL:** BIOS reset may be required

**Time:** 20-30 minutes

**Note:** This is for **LAPTOPS only**. Desktop computers typically don't have built-in fingerprint readers.

### ⚠️ CRITICAL: BIOS Reset (May Be Required)

Some fingerprint readers require BIOS reset:
1. Restart → Press **F10** during boot
2. Navigate: **Security → Fingerprint Reader Reset**
3. Enable reset on next boot
4. Save and restart

### Step 1: Verify Fingerprint Reader Detection

```bash
lsusb | grep -i "fingerprint\|validity"
```

### Step 2: Install Required Packages

```bash
yay -S python-validity-git
pacman -S fprintd
```

### Step 3: Configure udev Rules

```bash
sudo tee /etc/udev/rules.d/60-validity.rules << 'EOF'
SUBSYSTEMS=="usb", ATTRS{idVendor}=="138a", MODE="0666"
EOF

sudo udevadm control --reload
sudo usermod -a -G input $USER
```

### Step 4: Start Service

```bash
sudo systemctl enable --now python3-validity
```

### Step 5: Enroll Fingerprint

```bash
fprintd-enroll $USER
```

**SUCCESS:** Fingerprint reader configured and working

**Official Resources:**
- [ArchWiki: Fprint](https://wiki.archlinux.org/title/Fprint)
- [python-validity GitHub](https://github.com/uunicorn/python-validity)

**Next:** Continue with other configuration steps or [Exit & Reboot](#exit-reboot)

---

## SSH Server

**Purpose:** Install and configure SSH server with custom port and security. For comprehensive setup instructions and security best practices, refer to the [ArchWiki on OpenSSH](https://wiki.archlinux.org/title/OpenSSH).

**Prerequisites:**
- After first boot (not in chroot)
- User account created (module `user-creation.md`)

**Time:** 5-10 minutes

**ENVIRONMENT:** After first boot

### Step 1: Install SSH Server

```bash
pacman -S openssh
```

### Step 2: Configure SSH

```bash
sudo nano /etc/ssh/sshd_config
```

**Key settings:**
- `Port 1991` - Custom SSH port
- `PermitRootLogin no` - Disable root login
- `PasswordAuthentication yes` - Allow password auth

### Step 3: Enable SSH Service

```bash
sudo systemctl enable --now sshd
```

**SUCCESS:** SSH server running on port 1991, root login disabled

**Official Resources:**
- [ArchWiki: OpenSSH](https://wiki.archlinux.org/title/OpenSSH)

**Next:** [UFW Firewall](#ufw-firewall) or [Exit & Reboot](#exit-reboot)

---

## UFW Firewall

**Purpose:** Configure UFW firewall (all outgoing, only SSH incoming). For detailed configuration options and security considerations, consult the [ArchWiki on Uncomplicated Firewall (UFW)](https://wiki.archlinux.org/title/Uncomplicated_Firewall).

**Prerequisites:**
- After first boot (not in chroot)
- SSH server configured (module `ssh-server.md`)

**Time:** 5-10 minutes

**ENVIRONMENT:** After first boot

### Step 1: Install UFW

```bash
pacman -S ufw
```

### Step 2: Configure Defaults

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### Step 3: Allow SSH

```bash
sudo ufw allow 1991/tcp
```

### Step 4: Enable Firewall

```bash
sudo ufw enable
```

**SUCCESS:** UFW firewall active, SSH (1991) allowed, all outgoing allowed

**Official Resources:**
- [ArchWiki: UFW](https://wiki.archlinux.org/title/Uncomplicated_Firewall)

**Next:** [Fail2ban](#fail2ban) or [Exit & Reboot](#exit-reboot)

---

## Fail2ban

**Purpose:** Install fail2ban to prevent SSH brute force attacks. For in-depth configuration and usage, refer to the [ArchWiki on Fail2ban](https://wiki.archlinux.org/title/Fail2ban) and the [Fail2ban GitHub repository](https://github.com/fail2ban/fail2ban).

**Prerequisites:**
- After first boot (not in chroot)
- SSH server configured (module `ssh-server.md`)
- UFW firewall configured (module `ufw-firewall.md`)

**Time:** 5-10 minutes

**ENVIRONMENT:** After first boot

### Step 1: Install Fail2ban

```bash
pacman -S fail2ban
```

### Step 2: Configure SSH Jail

```bash
sudo tee /etc/fail2ban/jail.d/sshd.conf << 'EOF'
[sshd]
enabled = true
port = 1991
filter = sshd
backend = systemd
journalmatch = _SYSTEMD_UNIT=sshd.service
maxretry = 3
bantime = 3600
findtime = 600
EOF
```

### Step 3: Enable Fail2ban

```bash
sudo systemctl enable --now fail2ban
```

**SUCCESS:** Fail2ban protecting SSH, bans IPs after 3 failed attempts

**Official Resources:**
- [ArchWiki: Fail2ban](https://wiki.archlinux.org/title/Fail2ban)

**Next:** Continue with other configuration steps or [Exit & Reboot](#exit-reboot)

---

# GPU Drivers

## NVIDIA GPU Driver Installation

**Purpose:** Install proprietary NVIDIA drivers for optimal performance on systems with NVIDIA graphics cards. For comprehensive installation and configuration details, consult the [ArchWiki on NVIDIA](https://wiki.archlinux.org/title/NVIDIA).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see [User Creation](#user-creation))
- Network connectivity (see [NetworkManager](#networkmanager))
- Xorg Display Server installed (see [Xorg Display Server Configuration](#xorg-display-server-configuration)) or a Wayland compositor with NVIDIA EGLStreams support.
- Kernel headers installed (should be present if `linux-headers` was installed during core installation).

**Time:** 10-20 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

### Step 1: Blacklist Nouveau (Open-Source Driver)

The open-source `nouveau` driver can conflict with the proprietary NVIDIA drivers. It's best to blacklist it.

```bash
sudo tee /etc/modprobe.d/blacklist-nouveau.conf << EOF
blacklist nouveau
options nouveau modeset=0
EOF
```

After blacklisting, you need to regenerate your initramfs.

```bash
sudo mkinitcpio -P
```

---

### Step 2: Install NVIDIA Drivers

Install the main NVIDIA driver package and utilities.

```bash
sudo pacman -S nvidia nvidia-utils nvidia-settings
```

**Package breakdown:**
- `nvidia`: The core NVIDIA graphics driver.
- `nvidia-utils`: Essential NVIDIA utilities.
- `nvidia-settings`: GUI tool for configuring NVIDIA driver settings.

**Note on Kernel Versions:**
- If you are running a `linux-lts` kernel, you might need to install `nvidia-lts` instead of `nvidia`.
- If you use a custom or other kernel, ensure you install the corresponding `nvidia-dkms` package and have `dkms` installed.

---

### Step 3: Configure Xorg (Optional, but Recommended)

It's often good practice to explicitly tell Xorg to use the NVIDIA driver.

```bash
sudo nvidia-xconfig
```

This will generate an `/etc/X11/xorg.conf` file. If you already have a custom `xorg.conf` or snippets in `xorg.conf.d`, merge these changes carefully.

---

### Step 4: Reboot

Reboot your system to ensure the Nouveau driver is blacklisted and the NVIDIA drivers are loaded correctly.

```bash
reboot
```

---

### Step 5: Verify Installation

After reboot and logging into your desktop environment, you can verify the drivers are working.

```bash
nvidia-smi
```
This command should display information about your NVIDIA GPU.

You can also open `nvidia-settings` (usually from your application launcher) to further configure your display.

---

**SUCCESS:** NVIDIA proprietary drivers installed and configured.

**Next:** Optimize your display settings with `nvidia-settings` or proceed with other post-installation tasks.

---

## AMD GPU Driver Installation

**Purpose:** Ensure proper setup for AMD graphics cards, typically using the open-source `amdgpu` driver which is included in the Linux kernel. This module focuses on verifying setup and installing optional utilities. For detailed information, refer to the [ArchWiki on AMDGPU](https://wiki.archlinux.org/title/AMDGPU) and [ArchWiki on Vulkan](https://wiki.archlinux.org/title/Vulkan).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see [User Creation](#user-creation))
- Network connectivity (see [NetworkManager](#networkmanager))
- Xorg Display Server installed (see [Xorg Display Server Configuration](#xorg-display-server-configuration)) or a Wayland compositor.
- Kernel headers installed (should be present if `linux-headers` was installed during core installation).

**Time:** 5-10 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

### Step 1: Verify `amdgpu` Driver

The `amdgpu` open-source driver is typically included and loaded automatically by the Linux kernel for most modern AMD GPUs.

You can verify if the driver is loaded:

```bash
lspci -k | grep -EA3 'VGA|3D|Display'
```

Look for a line similar to `Kernel driver in use: amdgpu`. If you see this, the driver is likely working.

---

### Step 2: Install Vulkan Drivers and Other Utilities

For optimal performance and compatibility with modern games and applications, install the Vulkan drivers and a few essential utilities.

```bash
sudo pacman -S amdvlk vulkan-radeon libva-mesa-driver mesa-vdpau mesa-opencl opencl-amd
```

**Package breakdown:**
- `amdvlk`: AMD's official Vulkan driver (open-source).
- `vulkan-radeon`: Radeon Vulkan driver (part of Mesa, also open-source). You can choose one or both. `vulkan-radeon` is often preferred for gaming performance.
- `libva-mesa-driver`: VA-API (Video Acceleration API) for Mesa, enables hardware video decoding.
- `mesa-vdpau`: VDPAU (Video Decode and Presentation API for Unix) for Mesa, another hardware video decoding API.
- `mesa-opencl`: OpenCL support for Mesa drivers.
- `opencl-amd`: AMD's official OpenCL driver for ROCm-compatible GPUs.

---

### Step 3: Install `xorg-xrandr` (for Xorg)

If you are using Xorg, `xorg-xrandr` is a useful utility for display configuration.

```bash
sudo pacman -S xorg-xrandr
```

---

### Step 4: Configure Xorg (Optional)

In most cases, with modern AMD GPUs and the `amdgpu` driver, explicit Xorg configuration is not required. However, if you encounter issues, you might need a minimal `xorg.conf` file.

Example `/etc/X11/xorg.conf.d/20-amdgpu.conf`:
```
Section "OutputClass"
    Identifier "AMDgpu"
    MatchDriver "amdgpu"
    Driver "amdgpu"
EndSection
```
This is generally not needed.

---

### Step 5: Reboot (if needed)

If you installed new drivers or utilities, a reboot might be beneficial to ensure everything is loaded correctly, especially if you had issues with the `amdgpu` driver initially.

```bash
reboot
```

---

### Step 6: Verify Installation

After reboot and logging into your desktop environment, you can verify:

```bash
glxinfo -B # Requires 'mesa-utils' to be installed.
```
This command should show `OpenGL renderer string: AMD ...` and indicate direct rendering.

You can also check Vulkan support:
```bash
vulkaninfo # Requires 'vulkan-tools' to be installed.
```

---

**SUCCESS:** AMD GPU drivers and utilities installed and configured.

**Next:** Optimize your display settings or proceed with other post-installation tasks.

---

# Desktop Environments

## Xorg Display Server Configuration

**Purpose:** Install and configure the Xorg Display Server, a foundational component for many desktop environments. For an in-depth understanding and advanced configurations, refer to the [ArchWiki on Xorg](https://wiki.archlinux.org/title/Xorg).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see [User Creation](#user-creation))
- Network connectivity (see [NetworkManager](#networkmanager))
- (Optional) Desktop environment installed (e.g., GNOME, KDE, XFCE)

**Time:** 5-10 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

### Step 1: Install Xorg Server and Utilities

```bash
sudo pacman -S xorg-server xorg-xinit xorg-apps xorg-drivers
```

**Package breakdown:**
- `xorg-server`: The Xorg display server itself.
- `xorg-xinit`: Tools for manually starting an Xorg session (e.g., `startx`).
- `xorg-apps`: A collection of basic Xorg applications and utilities.
- `xorg-drivers`: Common open-source display drivers. You may need specific proprietary drivers later (e.g., NVIDIA, AMDGPU-PRO).

---

### Step 2: Install Essential Xorg Fonts

```bash
sudo pacman -S xorg-fonts-misc ttf-dejavu ttf-liberation
```

**Package breakdown:**
- `xorg-fonts-misc`: Miscellaneous Xorg fonts.
- `ttf-dejavu`: A font family designed for good readability.
- `ttf-liberation`: Another common and well-hinted font family.

---

### Step 3: Configure Input Devices (Optional, but Recommended)

Install `xf86-input-libinput` for modern input device handling. If you have specific needs for other input drivers (e.g., Wacom tablets), install them separately.

```bash
sudo pacman -S xf86-input-libinput
```

---

### Step 4: Test Xorg Installation

You can try to start a very basic Xorg session.

```bash
startx
```

This should launch a simple X session with a terminal. To exit, type `exit` in the terminal.

---

**SUCCESS:** Xorg Display Server installed. You can now use display managers (GDM, SDDM, LightDM) or `startx` to launch graphical environments.

**Next:** Install a Desktop Environment (if not already done) or configure a Window Manager.

---

## Wayland Display Server Configuration

**Purpose:** Install and configure a basic Wayland display server setup, often through a compositor like Sway or a desktop environment that uses Wayland (e.g., GNOME, KDE). For a comprehensive overview, refer to the [ArchWiki on Wayland](https://wiki.archlinux.org/title/Wayland) and specific compositors like [Sway](https://wiki.archlinux.org/title/Sway).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see [User Creation](#user-creation))
- Network connectivity (see [NetworkManager](#networkmanager))
- (Optional) Desktop environment installed (e.g., GNOME or KDE already provides Wayland support)

**Time:** 5-15 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

### Step 1: Install a Wayland Compositor (Example: Sway)

For a minimal Wayland setup, you typically install a compositor. Sway is a popular tiling Wayland compositor that is a drop-in replacement for i3.

```bash
sudo pacman -S sway swaylock swayidle dmenu foot
```

**Package breakdown (for Sway example):**
- `sway`: The Wayland compositor itself.
- `swaylock`: Screen locker for Sway.
- `swayidle`: Idleness management for Sway.
- `dmenu`: A dynamic menu for launching applications (often used with Sway).
- `foot`: A fast, lightweight Wayland terminal emulator.

**Note:** If you are using GNOME or KDE, they come with their own Wayland compositors (Mutter for GNOME, KWin for KDE) and display managers that handle Wayland sessions automatically. You generally don't need to install a separate compositor like Sway in that case, but these instructions are for a more minimal setup or for users who prefer tiling window managers.

---

### Step 2: Install Essential Wayland Fonts

```bash
sudo pacman -S ttf-dejavu ttf-liberation
```

**Package breakdown:**
- `ttf-dejavu`: A font family designed for good readability.
- `ttf-liberation`: Another common and well-hinted font family.

---

### Step 3: Configure Input Devices

Wayland compositors typically handle input devices automatically, but ensuring `libinput` is installed is good practice.

```bash
sudo pacman -S libinput
```

---

### Step 4: Start a Wayland Session (Example: Sway)

To start Sway from a TTY, you would typically use `exec sway` in your `.bash_profile` or `startsway` in your `.bashrc`.

Alternatively, if you installed a display manager like GDM or SDDM (with GNOME or KDE), you can select the Wayland session from the login screen. For Sway with a display manager, you might need additional configuration or a specific login entry.

For a manual start from TTY:

1.  Switch to a TTY (Ctrl+Alt+F2 to F6).
2.  Log in as your user.
3.  Execute: `sway`

This should launch your Sway session.

---

**SUCCESS:** Basic Wayland display server (via Sway) installed and configured.

**Next:** Customize your Wayland compositor, install a display manager (if using Sway and want graphical login), or install a full desktop environment that uses Wayland.

---

# Essential Applications

## Essential Applications

**Purpose:** Install a set of common and essential applications for daily use. For a broader selection of software, consult the [ArchWiki on Applications](https://wiki.archlinux.org/title/List_of_applications). Specific applications like [Firefox](https://wiki.archlinux.org/title/Firefox) and [LibreOffice](https://wiki.archlinux.org/title/LibreOffice) also have their own ArchWiki pages.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see [User Creation](#user-creation))
- Network connectivity (see [NetworkManager](#networkmanager))
- (Optional) Desktop environment installed (e.g., GNOME, KDE, XFCE)

**Time:** 10-20 minutes (depending on selected applications and internet speed)

**ENVIRONMENT:** After first boot (logged in as user)

---

### Step 1: Install a Web Browser

A web browser is fundamental for internet access. Firefox is a popular open-source choice.

```bash
sudo pacman -S firefox
```

---

### Step 2: Install a Text Editor / IDE

Choose a text editor or Integrated Development Environment (IDE) based on your preference.

**Option A: Visual Studio Code (via AUR helper)**
VS Code is a popular choice for development, but it's available in the AUR. You'll need an AUR helper (like `yay`) installed.

```bash
yay -S visual-studio-code-bin
```

**Option B: Neovim (Terminal-based text editor)**
Neovim is a powerful and highly configurable terminal-based text editor.

```bash
sudo pacman -S neovim
```

**Option C: Nano (Simple terminal-based text editor)**
Nano is a very simple and user-friendly terminal-based text editor. It's often already installed.

```bash
sudo pacman -S nano
```

---

### Step 3: Install an Office Suite

LibreOffice is a free and open-source office suite, a popular alternative to Microsoft Office.

```bash
sudo pacman -S libreoffice-still
```

**Note:** `libreoffice-fresh` is also available for the latest features, while `libreoffice-still` offers greater stability.

---

### Step 4: Install a Terminal Emulator (if not part of DE)

If you're running a minimal setup without a full desktop environment, you might need a dedicated terminal emulator. If using a DE, it likely comes with one (e.g., GNOME Terminal, Konsole, Xfce Terminal).

```bash
sudo pacman -S alacritty # or kitty, terminator, etc.
```

---

### Step 5: Install a File Manager (if not part of DE)

Similar to terminal emulators, if you're not using a full DE, you might want a standalone file manager.

```bash
sudo pacman -S thunar # or pcmanfm, nemo, etc.
```

---

### Step 6: Install an Archiving Tool

```bash
sudo pacman -S zip unzip p7zip unrar
```

---

**SUCCESS:** Essential applications installed.

**Next:** Explore other software from the official repositories or the AUR to further customize your system.

---

# Backup Solutions

## Timeshift for System Snapshots

**Purpose:** Install and configure Timeshift to create and manage system snapshots, providing an easy way to restore your system to a previous working state. For detailed information, refer to the [ArchWiki on Timeshift](https://wiki.archlinux.org/title/Timeshift) and the [Timeshift GitHub repository](https://github.com/teejee2008/timeshift).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see [User Creation](#user-creation))
- Network connectivity (see [NetworkManager](#networkmanager))
- A separate partition or disk for snapshots is highly recommended, especially for Btrfs snapshots. For rsync snapshots, any sufficiently large drive can be used.

**Time:** 5-15 minutes (initial setup), plus snapshot creation time.

**ENVIRONMENT:** After first boot (logged in as user)

---

### Step 1: Install Timeshift

Timeshift is available in the official Arch Linux repositories.

```bash
sudo pacman -S timeshift
```

---

### Step 2: Configure Timeshift (GUI)

Timeshift is primarily a GUI tool. After installation, launch it from your application launcher.

1.  **Launch Timeshift:** Open your application launcher and search for "Timeshift".
2.  **Select Snapshot Type:**
    *   **RSYNC:** Good for any filesystem. Creates a full backup initially, then incremental backups. Requires more space but is flexible.
    *   **BTRFS:** Highly efficient for Btrfs filesystems. Creates subvolume snapshots, which are very fast and use less space. **Recommended if your root filesystem is Btrfs.**
3.  **Select Snapshot Location:**
    *   Choose a separate partition or external drive. **Do NOT save snapshots on the same partition as your root filesystem.**
    *   For Btrfs, you typically select the root Btrfs filesystem, and Timeshift will manage the snapshots as subvolumes.
4.  **Schedule Snapshots:** Configure the frequency (e.g., daily, daily) and retention policy.
5.  **Create First Snapshot:** Once configured, create your first snapshot immediately.

---

### Step 3: Command Line Usage (Optional)

While Timeshift is GUI-focused, you can also manage snapshots from the command line after initial setup.

```bash
timeshift --help             # Display help and available commands
timeshift --check            # Check system for issues before snapshot
timeshift --create --comments "Manual snapshot" --tags D # Create a snapshot
timeshift --list             # List existing snapshots
timeshift --restore          # Restore from a snapshot (requires reboot into live environment)
```

**Note on restoring:** For restoring a system snapshot, it is often safer and recommended to boot from a live Arch Linux USB environment, install Timeshift there, and then use it to restore the snapshot. This ensures no system files are in use during the restore process.

---

**SUCCESS:** Timeshift installed and configured for system snapshots. Regularly create and verify your snapshots.

**Next:** Consider configuring personal data backups (e.g., using `rsync` or cloud sync services) as Timeshift is primarily for system files.

---

# Terminal Tools

## w3m Terminal Web Browser

**Purpose:** Install and configure `w3m`, a text-based web browser, essential for accessing web content in a terminal environment, especially useful in minimal installations or for troubleshooting.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see [User Creation](#user-creation))
- Network connectivity (see [NetworkManager](#networkmanager))
- (Optional) Terminal emulator (if not using default system console)

**Time:** 2-5 minutes

**ENVIRONMENT:** After first boot (logged in as user) or directly from TTY

---

## What is w3m?

`w3m` is a text-based web browser as well as a pager like `more` or `less`. It can display web pages in a terminal or console, making it invaluable for systems without a graphical desktop environment, for remote server administration, or for quick lookup of documentation during troubleshooting where a full browser is unavailable or undesirable. For more detailed information, refer to the [ArchWiki on w3m](https://wiki.archlinux.org/title/W3m).

---

## Step 1: Install w3m

`w3m` is available in the official Arch Linux repositories.

```bash
sudo pacman -S w3m
```

---

## Step 2: Basic Usage

After installation, you can launch `w3m` from your terminal or TTY.

```bash
w3m archlinux.org
```

**Key w3m Commands:**
*   **Arrow keys:** Scroll up/down, left/right.
*   **Tab / Shift+Tab:** Navigate between links.
*   **Enter:** Follow a link.
*   **B:** Go back.
*   **Shift+G:** Go to URL (prompts for a new URL).
*   **Q:** Quit `w3m`.

---

## Troubleshooting

### Problem: w3m cannot connect to websites
**Solution:**
1. **Verify network connectivity:** `ping archlinux.org`
2. **Check DNS resolution:** `dig archlinux.org`
3. **Check firewall rules:** Ensure outgoing connections are not blocked by UFW or other firewalls.

---

**SUCCESS:** `w3m` terminal web browser installed and ready for use.

**Official Resources:**
- [ArchWiki: w3m](https://wiki.archlinux.org/title/W3m)
- `man w3m` (consult the manual page for comprehensive options)

---

# Desktop Environments

## GNOME Desktop Environment

**Purpose:** Install and configure the GNOME Desktop Environment. For an in-depth guide on GNOME, refer to the [ArchWiki on GNOME](https://wiki.archlinux.org/title/GNOME) and its display manager, [GDM](https://wiki.archlinux.org/title/GDM).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see [User Creation](#user-creation))
- Network connectivity (see [NetworkManager](#networkmanager))

**Time:** 15-30 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

### Step 1: Install GNOME Packages

```bash
sudo pacman -S gnome gnome-extra gnome-tweaks nautilus-share
```

**Package breakdown:**
- `gnome`: The core GNOME desktop environment.
- `gnome-extra`: Additional GNOME applications (e.g., Maps, Weather, Boxes).
- `gnome-tweaks`: Utility for fine-tuning GNOME settings.
- `nautilus-share`: Enables file sharing through Nautilus.

---

### Step 2: Enable Display Manager

GNOME uses GDM (GNOME Display Manager) by default.

```bash
sudo systemctl enable gdm.service
```

---

### Step 3: Reboot

Reboot your system to start GNOME.

```bash
reboot
```

After reboot, you should be greeted by the GDM login screen. Log in with your user account.

---

**SUCCESS:** GNOME Desktop Environment installed and configured.

**Next:** Continue with other configuration modules or customize your GNOME experience.

---

## KDE Plasma Desktop Environment

**Purpose:** Install and configure the KDE Plasma Desktop Environment. For a comprehensive guide on KDE Plasma, refer to the [ArchWiki on KDE](https://wiki.archlinux.org/title/KDE) and its recommended display manager, [SDDM](https://wiki.archlinux.org/title/SDDM).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see [User Creation](#user-creation))
- Network connectivity (see [NetworkManager](#networkmanager))

**Time:** 15-30 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

### Step 1: Install KDE Plasma Packages

```bash
sudo pacman -S plasma kde-applications sddm
```

**Package breakdown:**
- `plasma`: The core KDE Plasma desktop environment.
- `kde-applications`: A meta-package containing many common KDE applications.
- `sddm`: Simple Desktop Display Manager, recommended for KDE Plasma.

---

### Step 2: Enable Display Manager

KDE Plasma typically uses SDDM (Simple Desktop Display Manager).

```bash
sudo systemctl enable sddm.service
```

---

### Step 3: Reboot

Reboot your system to start KDE Plasma.

```bash
reboot
```

After reboot, you should be greeted by the SDDM login screen. Log in with your user account.

---

**SUCCESS:** KDE Plasma Desktop Environment installed and configured.

**Next:** Continue with other configuration modules or customize your KDE Plasma experience.

---

## XFCE Desktop Environment

**Purpose:** Install and configure the XFCE Desktop Environment. For an in-depth guide on XFCE, refer to the [ArchWiki on XFCE](https://wiki.archlinux.org/title/Xfce) and its common display manager, [LightDM](https://wiki.archlinux.org/title/LightDM).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see [User Creation](#user-creation))
- Network connectivity (see [NetworkManager](#networkmanager))

**Time:** 10-20 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

### Step 1: Install XFCE Packages

```bash
sudo pacman -S xfce4 xfce4-goodies lightdm lightdm-gtk-greeter
```

**Package breakdown:**
- `xfce4`: The core XFCE desktop environment.
- `xfce4-goodies`: A collection of useful plugins and applications for XFCE.
- `lightdm`: A lightweight display manager.
- `lightdm-gtk-greeter`: The GTK greeter for LightDM.

---

### Step 2: Enable Display Manager

XFCE typically uses LightDM (Light Display Manager).

```bash
sudo systemctl enable lightdm.service
```

---

### Step 3: Reboot

Reboot your system to start XFCE.

```bash
reboot
```

After reboot, you should be greeted by the LightDM login screen. Log in with your user account.

---

**SUCCESS:** XFCE Desktop Environment installed and configured.

**Next:** Continue with other configuration modules or customize your XFCE experience.

---

## Exit & Reboot

**Purpose:** Exit chroot environment, unmount partitions, and reboot system

**Prerequisites:**
- All configuration steps completed
- System ready for first boot

**Time:** 5 minutes

### Step 1: Exit Chroot Environment

```bash
exit
```

**Prompt changes back to:**
```
root@archiso ~ #
```

**SUCCESS:** You are now back in Arch Linux live USB environment

### Step 2: Unmount All Partitions

```bash
# Unmount all mounted filesystems recursively
umount -R /mnt
```

**If error "target is busy":**
```bash
# Force unmount
umount -lR /mnt
```

### Step 3: Close Encrypted Volumes (if used)

```bash
# Close encrypted swap
cryptsetup close cryptswap

# Close encrypted root
cryptsetup close cryptroot
```

**Verify all closed:**
```bash
ls /dev/mapper/

# Should show only:
# control
```

### Step 4: Reboot System

```bash
reboot
```

**Remove USB drive** when system shuts down (before it boots again).

---

**SUCCESS:** System ready for first boot

**After reboot:**
- GRUB menu appears
- Select Arch Linux
- Enter LUKS passphrase (if not using keyfile)
- Login with created user
- Continue with post-installation configuration

---

**Installation Complete!**

---

**Back to:** [Home](HOME.md) | **Previous:** [Core Installation](CORE-INSTALLATION.md)

# Post-Installation Configuration - Intel Hardware

**Purpose:** Configure system after core installation

**Prerequisites:**
- Core installation completed (see [Core Installation](02-CORE-INSTALLATION.md))
- Inside chroot environment

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
- **[Intel Graphics Drivers](#intel-graphics-drivers)** - Intel graphics (Intel version only)
- **[AMD Graphics Drivers](#amd-graphics-drivers)** - AMD graphics (AMD version only)
- **[Exit & Reboot](#exit-reboot)** - Final step

---

## Locale & Timezone

**Purpose:** Configure system locale and timezone

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

**Purpose:** Create non-root user account with sudo access

**Prerequisites:**
- Inside chroot environment
- Sudo package installed (included in core installation)

**Time:** 2-3 minutes

### Step 1: Create User

```bash
# Replace "username" with your desired username
useradd -m -G wheel,video,audio,storage,input,power,network -s /bin/bash username

# Explanation:
# -m: Create home directory (/home/username)
# -G: Add to supplementary groups:
#     wheel: sudo access
#     video: GPU access
#     audio: sound access
#     storage: removable media access
#     input: input devices
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

**Purpose:** Install and configure GRUB bootloader

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

**Purpose:** Configure automatic LUKS decryption on boot using keyfile

**Prerequisites:**
- Inside chroot environment
- LUKS encryption configured (see [Pre-Installation: LUKS Encryption](01-PRE-INSTALLATION.md#luks-encryption))
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

**Purpose:** Enable NetworkManager for network connectivity

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

**Purpose:** Install and configure WiFi support

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

**Purpose:** Install and enable Bluetooth support

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

**Purpose:** Install and configure audio server (PipeWire)

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

## Intel Graphics Drivers

**Purpose:** Install Intel graphics drivers (Mesa, Vulkan, VA-API)

**Prerequisites:**
- Inside chroot environment
- **Note:** This is only for Intel version. See [Intel branch](../core-intel/wiki/03-POST-INSTALLATION.md#intel-graphics-drivers)

**Time:** 3-5 minutes

### Step 1: Install Intel Graphics Drivers

```bash
# Install Intel graphics drivers
pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel \
  xf86-video-intel intel-media-driver
```

**Package breakdown:**
- `mesa` - OpenGL implementation
- `lib32-mesa` - 32-bit OpenGL (for compatibility)
- `vulkan-intel` - Vulkan driver for Intel
- `lib32-vulkan-intel` - 32-bit Vulkan
- `xf86-video-intel` - X11 Intel driver (legacy)
- `intel-media-driver` - Hardware video acceleration

**SUCCESS:** Intel graphics drivers installed

**Official Resources:**
- [Intel Graphics on ArchWiki](https://wiki.archlinux.org/title/Intel_graphics)
- [Mesa 3D Graphics Library](https://www.mesa3d.org/)

**Next:** Continue with other configuration steps or [Exit & Reboot](#exit-reboot)

---

## AMD Graphics Drivers

**Purpose:** Install AMD graphics drivers (Mesa, Vulkan, VA-API)

**Prerequisites:**
- Inside chroot environment
- **Note:** This is only for AMD version. See [AMD branch](../core-amd/wiki/03-POST-INSTALLATION.md#amd-graphics-drivers)

**Time:** 3-5 minutes

### Step 1: Install AMD Graphics Drivers

```bash
# Install AMD graphics drivers
pacman -S mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon \
  xf86-video-amdgpu
```

**Package breakdown:**
- `mesa` - OpenGL implementation
- `lib32-mesa` - 32-bit OpenGL (for compatibility)
- `vulkan-radeon` - Vulkan driver for AMD Radeon
- `lib32-vulkan-radeon` - 32-bit Vulkan
- `xf86-video-amdgpu` - X11 AMD driver

**SUCCESS:** AMD graphics drivers installed

**Official Resources:**
- [AMD Graphics on ArchWiki](https://wiki.archlinux.org/title/AMDGPU)
- [Mesa 3D Graphics Library](https://www.mesa3d.org/)

**Next:** Continue with other configuration steps or [Exit & Reboot](#exit-reboot)

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

**Back to:** [Home](00-HOME.md) | **Previous:** [Core Installation](02-CORE-INSTALLATION.md)

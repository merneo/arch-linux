# Post-Installation Checklist

**Purpose:** Checklist for what to do after your first successful boot into Arch Linux. This guide helps you complete the setup and ensure everything is working correctly.

**Quick Links:**
- [Installation Checklist](INSTALLATION_CHECKLIST.md) - Installation progress
- [Troubleshooting](TROUBLESHOOTING.md) - If you encounter problems
- [Comparison Guide](COMPARISON_GUIDE.md) - Choose desktop environment

---

## First Boot Verification

### Boot Process
- [ ] GRUB menu appears
- [ ] Can select Arch Linux entry
- [ ] LUKS passphrase prompt works (if encrypted)
- [ ] System boots without errors
- [ ] Login screen appears

### Initial Login
- [ ] Can login with created user
- [ ] Terminal opens correctly
- [ ] Basic commands work (`ls`, `cd`, `pwd`)

---

## Network Configuration

### Ethernet
- [ ] Network connection active
- [ ] Can ping external sites: `ping archlinux.org`
- [ ] DNS resolution works: `nslookup archlinux.org`

### WiFi (If Using)
- [ ] WiFi adapter detected: `iwconfig` or `ip link`
- [ ] Can scan for networks: `nmcli device wifi list`
- [ ] Connected to WiFi network
- [ ] Internet access working

### NetworkManager
- [ ] NetworkManager service running: `systemctl status NetworkManager`
- [ ] NetworkManager enabled: `systemctl is-enabled NetworkManager`
- [ ] Can use `nmtui` or `nmcli` for network management

**If Issues:** See [Network Troubleshooting](TROUBLESHOOTING.md#-network-issues)

---

## System Updates

### Update System
- [ ] System updated: `sudo pacman -Syu`
- [ ] No errors during update
- [ ] System rebooted after update (if kernel updated)

**Important:** Always update before installing additional software!

---

## GPU Drivers

### NVIDIA GPU
- [ ] NVIDIA drivers installed: [NVIDIA Drivers Module](modules/nvidia-drivers.md)
- [ ] `nvidia-smi` works correctly
- [ ] Display output working
- [ ] Performance acceptable

**If Issues:** See [NVIDIA Troubleshooting](TROUBLESHOOTING.md#nvidia-gpu-issues)

### AMD GPU
- [ ] AMD drivers installed (if needed): [AMD Drivers Module](modules/amd-drivers.md)
- [ ] Display output working
- [ ] Performance acceptable

**Note:** AMD integrated graphics often work out-of-the-box.

### Intel Integrated Graphics
- [ ] Display output working
- [ ] Usually works without additional drivers

---

## Display Server

### Xorg
- [ ] Xorg installed: [Xorg Configuration](modules/xorg-config.md)
- [ ] Xorg starts correctly: `startx` (if not using display manager)
- [ ] Display resolution correct
- [ ] Input devices working (keyboard, mouse)

### Wayland
- [ ] Wayland compositor installed: [Wayland Configuration](modules/wayland-config.md)
- [ ] Wayland session starts correctly
- [ ] Display resolution correct
- [ ] Input devices working

**Choice:** See [Comparison Guide - Xorg vs Wayland](COMPARISON_GUIDE.md#xorg-vs-wayland)

---

## Desktop Environment

### GNOME
- [ ] GNOME installed: [GNOME Module](modules/gnome.md)
- [ ] GDM login screen appears
- [ ] Can login to GNOME session
- [ ] Desktop environment working correctly
- [ ] Applications launch correctly

### KDE Plasma
- [ ] KDE Plasma installed: [KDE Plasma Module](modules/kde-plasma.md)
- [ ] SDDM login screen appears
- [ ] Can login to KDE session
- [ ] Desktop environment working correctly
- [ ] Applications launch correctly

### XFCE
- [ ] XFCE installed: [XFCE Module](modules/xfce.md)
- [ ] LightDM login screen appears (or configured display manager)
- [ ] Can login to XFCE session
- [ ] Desktop environment working correctly
- [ ] Applications launch correctly

### Window Managers
- [ ] i3 installed: [i3 Window Manager](modules/i3wm.md)
- [ ] Hyprland installed: [Hyprland](modules/hyprland.md)
- [ ] Window manager starts correctly
- [ ] Configuration working

**Choice:** See [Comparison Guide - Desktop Environments](COMPARISON_GUIDE.md#desktop-environments)

---

## Essential Applications

### Web Browser
- [ ] Web browser installed: [Essential Applications](modules/essential-applications.md)
- [ ] Browser opens correctly
- [ ] Can browse websites
- [ ] Downloads work

### Terminal Emulator
- [ ] Terminal emulator installed
- [ ] Terminal opens correctly
- [ ] Can run commands
- [ ] Colors and fonts correct

### File Manager
- [ ] File manager installed
- [ ] Can browse files
- [ ] Can create/delete files
- [ ] Permissions working correctly

### Text Editor
- [ ] Text editor installed (nano, vim, or GUI editor)
- [ ] Can create/edit files
- [ ] Syntax highlighting works (if applicable)

**See:** [Essential Applications Module](modules/essential-applications.md)

---

## Audio

### Audio Setup
- [ ] Audio working: [Audio Module](modules/audio.md)
- [ ] Can play sound (test with `aplay` or media player)
- [ ] Volume control works
- [ ] Microphone works (if needed)

**If Issues:** See [Audio Troubleshooting](TROUBLESHOOTING.md#-audio-issues)

---

## Laptop Hardware (If Applicable)

### Touchpad
- [ ] Touchpad working: [Touchpad Module](modules/touchpad.md)
- [ ] Touchpad responds to gestures
- [ ] Scrolling works
- [ ] Clicking works

### Webcam
- [ ] Webcam detected: [Webcam Module](modules/webcam.md)
- [ ] Webcam works in applications
- [ ] Video recording works (if needed)

### IR Camera (If Present)
- [ ] IR camera detected: [IR Camera Module](modules/ir-camera.md)
- [ ] Howdy installed and configured
- [ ] Face recognition works

### Fingerprint Reader (If Present)
- [ ] Fingerprint reader detected: [Fingerprint Module](modules/fingerprint.md)
- [ ] Fingerprints enrolled
- [ ] Fingerprint login works

**See:** [Hardware Phase](phases/HARDWARE.md)

---

## Security

### SSH (If Using)
- [ ] SSH server installed: [SSH Server Module](modules/ssh-server.md)
- [ ] SSH service running: `systemctl status sshd`
- [ ] Can connect via SSH (from another computer)
- [ ] SSH configured securely (port changed, root disabled)

### Firewall
- [ ] UFW installed: [UFW Firewall Module](modules/ufw-firewall.md)
- [ ] UFW enabled: `sudo ufw enable`
- [ ] UFW rules configured correctly
- [ ] Firewall blocking unwanted connections

### Fail2ban (If Using SSH)
- [ ] Fail2ban installed: [Fail2ban Module](modules/fail2ban.md)
- [ ] Fail2ban service running
- [ ] Fail2ban protecting SSH

**See:** [Security Phase](phases/SECURITY.md)

---

## Backup & Recovery

### Timeshift
- [ ] Timeshift installed: [Timeshift Module](modules/timeshift.md)
- [ ] First snapshot created
- [ ] Snapshot schedule configured
- [ ] Can restore from snapshot (tested)

### Btrfs Snapshots (If Using Btrfs)
- [ ] Btrfs snapshots working
- [ ] Snapshot schedule configured
- [ ] Can restore from snapshot

**See:** [Backup & Recovery Guide](BACKUP_RECOVERY.md)

---

## System Optimization

### Performance Tuning
- [ ] System performance acceptable
- [ ] Boot time reasonable
- [ ] Memory usage acceptable
- [ ] CPU usage normal

**See:** [Performance Tuning Guide](PERFORMANCE_TUNING.md)

### Package Management
- [ ] AUR helper installed (if using): `yay`, `paru`, etc.
- [ ] AUR helper working correctly
- [ ] Can install packages from AUR

---

## Customization

### System Settings
- [ ] System language/locale correct
- [ ] Timezone correct
- [ ] Keyboard layout correct
- [ ] Display settings configured

### Desktop Customization
- [ ] Desktop theme/appearance customized
- [ ] Wallpaper set
- [ ] Icons and fonts configured
- [ ] Window manager settings (if applicable)

### Applications
- [ ] Additional applications installed
- [ ] Applications configured
- [ ] Default applications set

---

## Final Verification

### System Health
- [ ] System boots reliably
- [ ] No error messages in logs: `journalctl -b`
- [ ] All services running correctly: `systemctl status`
- [ ] Disk space sufficient: `df -h`

### Functionality
- [ ] Network working
- [ ] Audio working
- [ ] Display working
- [ ] Input devices working
- [ ] Applications working

### Documentation
- [ ] System configuration documented (if needed)
- [ ] Important passwords/keys backed up
- [ ] Recovery procedures known

---

## Optional Enhancements

### Development Tools
- [ ] Development tools installed (if needed)
- [ ] Git configured
- [ ] Programming languages installed

### Media
- [ ] Media codecs installed
- [ ] Can play videos
- [ ] Can play music

### Gaming
- [ ] Gaming drivers installed (if needed)
- [ ] Steam installed (if gaming)
- [ ] Games working

---

## Troubleshooting Resources

If you encounter issues:

1. **Check Logs:**
   ```bash
   journalctl -b  # Current boot
   journalctl -xe  # Recent errors
   ```

2. **Verify Services:**
   ```bash
   systemctl status <service>
   ```

3. **Check Documentation:**
   - [Troubleshooting Guide](TROUBLESHOOTING.md)
   - [FAQ](FAQ.md)
   - [Common Mistakes](COMMON_MISTAKES.md)

4. **Get Help:**
   - [ArchWiki](https://wiki.archlinux.org/)
   - [Arch Linux Forums](https://bbs.archlinux.org/)

---

## Installation Complete! 🎉

**Congratulations!** Your Arch Linux system is now fully set up and ready to use.

**Next Steps:**
- Explore your new system
- Customize to your preferences
- Learn more about Arch Linux
- Contribute to this repository (if desired)

---

**Back to:** [Main Index](INDEX.md) | [Repository Root](README.md)

# Module: Laptop IR Camera Configuration (Face Recognition)

**Purpose:** Configure IR (Infrared) camera for face recognition (Howdy)

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`) OR after first boot
- Laptop hardware (not desktop)
- IR camera hardware present (usually found in business laptops)
- Webcam module (`18-laptop-webcam.md`) completed

**Time:** 15-30 minutes

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot

**Note:** This module is for **LAPTOPS only**. Desktop computers typically don't have built-in IR cameras. IR cameras are used for face recognition (Windows Hello, Howdy, etc.).

---

## Step 1: Verify IR Camera Detection

Verifying that your system correctly detects the IR camera involves checking USB devices and the `/dev/video*` interfaces. For more details on listing USB devices, refer to the [ArchWiki on lsusb](https://wiki.archlinux.org/title/USB#lsusb). IR cameras typically appear as `/dev/videoX`, where `X` is a number, distinct from a standard webcam.

```bash
# List USB devices
lsusb | grep -i "ir\|camera"

# Expected output (example):
# Bus 001 Device 007: ID 04f2:b58e Chicony Electronics Co., Ltd HP IR Camera

# Check video devices
ls -la /dev/video*

# Expected output (example):
# /dev/video0  (HD webcam)
# /dev/video2  (IR camera)
```

---

## Step 2: Install Required Packages

The UVC (USB Video Class) driver for webcams is usually included in the Linux kernel and often handles IR cameras as well. `Howdy` is a face recognition program that uses IR cameras for PAM authentication.

```bash
# UVC driver (should be in kernel). For lsmod details, see [ArchWiki: Kernel modules](https://wiki.archlinux.org/title/Kernel_modules).
lsmod | grep uvcvideo

# Install Howdy for face recognition. Howdy is an AUR package, requiring an [AUR helper](https://wiki.archlinux.org/title/AUR_helpers) like yay.
yay -S howdy-bin

# Or build from source:
# yay -S howdy
```

---

## Step 3: Configure Howdy

```bash
# Edit Howdy configuration
sudo nano /lib/security/howdy/config.ini

# Find camera path and set to IR camera:
# device_path = /dev/video2

# Or use automatic detection:
# device_path = /dev/video*
```

---

## Step 4: Test IR Camera

```bash
# Test with Howdy
sudo howdy test

# Expected output shows camera feed
```

---

## Step 5: Enroll Face

```bash
# Enroll your face
sudo howdy add

# Follow prompts:
# 1. Look at camera
# 2. Wait for IR emitter to activate
# 3. Follow instructions to move head
# 4. Complete enrollment
```

---

## Step 6: Configure PAM (Optional)

[PAM (Pluggable Authentication Modules)](https://wiki.archlinux.org/title/PAM) is a system of libraries that handle the authentication tasks of applications and services. Configuring Howdy with PAM allows for face recognition authentication for various system operations like `sudo` or login managers.

**For sudo authentication:**

```bash
sudo nano /etc/pam.d/sudo
```

Add at the top:
```
auth      sufficient  pam_python.so /lib/security/howdy/pam.py
```

**For SDDM login:** If you are using SDDM as your display manager, you can integrate Howdy for login authentication. See [ArchWiki: SDDM](https://wiki.archlinux.org/title/SDDM).

```bash
sudo nano /etc/pam.d/sddm
```

Add at the top:
```
auth      sufficient  pam_python.so /lib/security/howdy/pam.py
```

---

## Troubleshooting

For more extensive troubleshooting on Howdy and IR camera issues, refer to the [ArchWiki on Howdy#Troubleshooting](https://wiki.archlinux.org/title/Howdy#Troubleshooting) and [ArchWiki on Webcam setup](https://wiki.archlinux.org/title/Webcam_setup).

### Problem: IR camera not detected
**Solution:**
1. Check USB: `lsusb | grep -i "ir\|camera"`
2. Verify video device: `ls -la /dev/video*`
3. Check kernel module: `lsmod | grep uvcvideo`
4. Check kernel messages: `dmesg | grep -i video`. For `dmesg` details, see [ArchWiki: dmesg](https://wiki.archlinux.org/title/Dmesg).

### Problem: IR emitter not activating
**Solution:**
1. Check if IR emitter is supported: `v4l2-ctl --list-devices`
2. Some laptops require specific kernel parameters
3. Check Howdy logs: `sudo journalctl -u howdy`. For `journalctl` details, see [ArchWiki: Systemd/Journal](https://wiki.archlinux.org/title/Systemd/Journal).

### Problem: Howdy enrollment fails
**Solution:**
1. Ensure good lighting
2. Remove glasses/hat if wearing
3. Check camera path in config.ini
4. Verify IR emitter is working

---

**SUCCESS:** IR camera configured and face recognition working

**Official Resources:**
- [Howdy GitHub](https://github.com/boltgolt/howdy)
- [ArchWiki: Howdy](https://wiki.archlinux.org/title/Howdy)
- [ArchWiki: Webcam Setup](https://wiki.archlinux.org/title/Webcam_setup)

**Next:** Continue with other configuration modules

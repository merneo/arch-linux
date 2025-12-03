# Module: Laptop Webcam Configuration

**Purpose:** Configure built-in webcam for laptops

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`) OR after first boot
- Laptop hardware (not desktop)
- Webcam hardware present

**Time:** 2-5 minutes

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot

**Note:** This module is for **LAPTOPS only**. Desktop computers typically don't have built-in webcams (use external USB webcams if needed).

---

## Step 1: Verify Webcam Detection

```bash
# List USB devices
lsusb | grep -i camera

# Expected output (example):
# Bus 001 Device 005: ID 04f2:b58f Chicony Electronics Co., Ltd HP HD Camera

# Check video devices
ls -la /dev/video*

# Expected output (example):
# /dev/video0  (HD webcam)
# /dev/video2  (IR camera, if present)
```

---

## Step 2: Install Required Packages

```bash
# UVC (USB Video Class) driver is usually included in kernel
# Verify it's loaded:
lsmod | grep uvcvideo

# If not loaded, install linux-firmware (should be in base installation):
pacman -S linux-firmware

# For testing webcam:
pacman -S v4l-utils

# For GUI applications:
pacman -S cheese  # GNOME webcam app
# OR
pacman -S kamoso   # KDE webcam app
```

---

## Step 3: Test Webcam

```bash
# Using v4l2-ctl (command line)
v4l2-ctl --list-devices

# Expected output shows webcam device:
# HP HD Camera (usb-0000:00:14.0-6):
#   /dev/video0

# Test capture (if ffmpeg is installed):
pacman -S ffmpeg
ffmpeg -f v4l2 -i /dev/video0 -frames:v 1 test.jpg

# Check if image was created:
ls -lh test.jpg
```

---

## Step 4: Configure Permissions (if needed)

```bash
# Add user to video group (if not already)
sudo usermod -a -G video $USER

# Log out and log back in for group changes to take effect
```

---

## Step 5: Test with GUI Application

```bash
# Using Cheese (GNOME):
cheese

# Using Kamoso (KDE):
kamoso

# Using mpv (media player):
pacman -S mpv
mpv av://v4l2:/dev/video0
```

---

## Troubleshooting

### Problem: Webcam not detected
**Solution:**
1. Check USB connection: `lsusb | grep -i camera`
2. Verify kernel module: `lsmod | grep uvcvideo`
3. Check dmesg: `dmesg | grep -i video`
4. Try different USB port (if external webcam)

### Problem: Permission denied
**Solution:**
1. Add user to video group: `sudo usermod -a -G video $USER`
2. Log out and log back in
3. Check permissions: `ls -la /dev/video*`

### Problem: Webcam works but application can't access it
**Solution:**
1. Check if another application is using the webcam: `lsof /dev/video0`
2. Close other applications using webcam
3. Verify application has permissions

---

**SUCCESS:** Webcam detected and working

**Official Resources:**
- [ArchWiki: Webcam Setup](https://wiki.archlinux.org/title/Webcam_setup)
- [UVC (USB Video Class) Driver](https://www.kernel.org/doc/html/latest/media/v4l-drivers/uvcvideo.html)

**Next:** Continue with other configuration modules

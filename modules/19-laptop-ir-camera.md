# Module: Laptop IR Camera Configuration (Face Recognition)

**Purpose:** Configure IR (Infrared) camera for face recognition (Howdy)

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`) OR after first boot
- Laptop hardware (not desktop)
- IR camera hardware present (usually found in business laptops)
- Webcam module (`18-laptop-webcam.md`) completed

**Time:** 15-30 minutes

**Note:** This module is for **LAPTOPS only**. Desktop computers typically don't have built-in IR cameras. IR cameras are used for face recognition (Windows Hello, Howdy, etc.).

---

## Step 1: Verify IR Camera Detection

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

```bash
# UVC driver (should be in kernel)
lsmod | grep uvcvideo

# Install Howdy for face recognition
# Note: Howdy requires AUR package
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

**For sudo authentication:**

```bash
sudo nano /etc/pam.d/sudo
```

Add at the top:
```
auth      sufficient  pam_python.so /lib/security/howdy/pam.py
```

**For SDDM login:**

```bash
sudo nano /etc/pam.d/sddm
```

Add at the top:
```
auth      sufficient  pam_python.so /lib/security/howdy/pam.py
```

---

## Troubleshooting

### Problem: IR camera not detected
**Solution:**
1. Check USB: `lsusb | grep -i "ir\|camera"`
2. Verify video device: `ls -la /dev/video*`
3. Check kernel module: `lsmod | grep uvcvideo`
4. Check dmesg: `dmesg | grep -i video`

### Problem: IR emitter not activating
**Solution:**
1. Check if IR emitter is supported: `v4l2-ctl --list-devices`
2. Some laptops require specific kernel parameters
3. Check Howdy logs: `sudo journalctl -u howdy`

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

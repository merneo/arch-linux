# Module: Laptop IR Camera Configuration (Face Recognition)

**Purpose:** Configure IR (Infrared) camera for face recognition using software like Howdy. An IR camera captures images using infrared light, making it effective in various lighting conditions and crucial for biometric authentication systems such as Windows Hello and its Linux counterpart, Howdy.

**Prerequisites:**
- Inside chroot environment (module `chroot.md`) OR after first boot
- Laptop hardware (not desktop)
- IR camera hardware present (usually found in business laptops)
- Webcam module (`webcam.md`) completed

**Time:** 15-30 minutes

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot

**Note:** This module is for **LAPTOPS only**. Desktop computers typically don't have built-in IR cameras.

---

## How to know if you have an IR Camera?

IR cameras are often physically distinct from regular webcams, sometimes marked with "Windows Hello" or similar branding. To definitively check:
-   **Physical inspection:** Look for an additional small sensor or emitter next to your standard webcam lens.
-   **Laptop specifications:** Check your laptop's model specifications online.
-   **System detection commands:** The `lsusb` output (detailed below) often shows distinct Vendor ID (VID) and Product ID (PID) for IR cameras. You might see terms like "IR Camera" or specific sensor names. IR cameras typically register as separate `/dev/videoX` devices from your main webcam.

---

## Step 1: Verify IR Camera Detection

Verifying that your system correctly detects the IR camera involves checking USB devices and the `/dev/video*` interfaces. For more details on listing USB devices, refer to the [ArchWiki on lsusb](https://wiki.archlinux.org/title/USB#lsusb).

```bash
# List USB devices. Look for entries containing "IR", "Infrared", "camera" along with vendor names.
lsusb | grep -i "ir\|camera"

# Expected output (example for HP):
# Bus 001 Device 007: ID 04f2:b58e Chicony Electronics Co., Ltd HP IR Camera
# Explanation: Note the unique Vendor ID (04f2) and Product ID (b58e) specific to the IR camera.

# Check video devices. An IR camera will typically appear as a separate /dev/videoX device,
# often with a higher index number than your regular webcam.
ls -la /dev/video*

# Expected output (example):
# crw-rw----+ 1 root video 81, 0 Jan  1 00:00 /dev/video0  (Likely your HD webcam)
# crw-rw----+ 1 root video 81, 2 Jan  1 00:00 /dev/video2  (This might be your IR camera)
```

---

## Step 2: Install Required Packages

Most IR cameras, like standard webcams, utilize the UVC (USB Video Class) driver which is part of the Linux kernel. `Howdy` is a face recognition program designed to integrate with PAM, offering biometric authentication. Its functionality is heavily dependent on specific IR camera models being supported.

```bash
# UVC driver (should be in kernel). For lsmod details, see [ArchWiki: Kernel modules](https://wiki.archlinux.org/title/Kernel_modules).
lsmod | grep uvcvideo

# Install Howdy for face recognition. Howdy is an AUR package, requiring an [AUR helper](https://wiki.archlinux.org/title/AUR_helpers) like yay.
# Ensure your specific IR camera model is supported by Howdy. Compatibility often depends on the camera's vendor and product ID.
yay -S howdy-bin

# Or build from source:
# yay -S howdy
```

---

## Step 3: Configure Howdy

The behavior of Howdy is controlled by its `config.ini` file. It's crucial to correctly specify the path to your IR camera device.

```bash
# Edit Howdy configuration
sudo nano /lib/security/howdy/config.ini
```

**Key Configuration Parameters:**

*   `device_path`: This parameter specifies the video device node corresponding to your IR camera. It's typically `/dev/videoX`, where `X` is the index of your IR camera. You identified this in Step 1. Using `/dev/video*` allows Howdy to try and auto-detect.
*   `face_detection_model`: Howdy might use different models for face detection; ensure it's compatible with your setup.
*   `detection_threshold`: Adjusts sensitivity for face detection.

**Find and modify these lines:**

```ini
# Find camera path and set to IR camera. Replace '/dev/videoX' with the path identified in Step 1.
device_path = /dev/video2

# Or use automatic detection if unsure and have only one IR camera:
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

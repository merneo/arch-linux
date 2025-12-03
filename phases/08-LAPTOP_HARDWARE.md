# Phase 08: Laptop Hardware Configuration

**Purpose:** Configure laptop-specific hardware (touchpad, webcam, IR camera, fingerprint)

**Prerequisites:**
- After first boot (not in chroot)
- Laptop hardware present

**Steps (execute as needed):**

1. **[Touchpad Configuration](../steps/17-laptop-touchpad.md)**
   - Configure touchpad/trackpad

2. **[Webcam Configuration](../steps/18-laptop-webcam.md)**
   - Configure built-in webcam

3. **[IR Camera Configuration](../steps/19-laptop-ir-camera.md)** (if IR camera present)
   - Configure IR camera for face recognition (Howdy)

4. **[Fingerprint Reader](../steps/20-laptop-fingerprint.md)** (if fingerprint reader present)
   - Configure fingerprint reader

## Verification

```bash
libinput list-devices | grep -i touchpad
lsusb | grep -i camera
lsusb | grep -i fingerprint
```

## Next Phase

**[Phase 09: Finalize](09-FINALIZE.md)**

# Phase 08: Hardware Configuration

**Purpose:** Configure laptop-specific hardware (touchpad, webcam, IR camera, fingerprint). This phase addresses the integration and configuration of specialized hardware often found in laptops, enhancing user experience and functionality. For general laptop-related information and common issues, refer to the [ArchWiki on Laptop](https://wiki.archlinux.org/title/Laptop).

**Prerequisites:**
- After first boot (not in chroot)
- Laptop hardware present

**Time Estimate:** 10-20 minutes
**Difficulty:** ⭐⭐ Intermediate

**Steps (execute as needed):**

1. **[Touchpad Configuration](../steps/touchpad.md)**
   - Configure touchpad/trackpad

2. **[Webcam Configuration](../steps/webcam.md)**
   - Configure built-in webcam

3. **[IR Camera Configuration](../steps/ir-camera.md)** (if IR camera present)
   - Configure IR camera for face recognition (Howdy)

4. **[Fingerprint Reader](../steps/fingerprint.md)** (if fingerprint reader present)
   - Configure fingerprint reader

## Verification

```bash
libinput list-devices | grep -i touchpad
lsusb | grep -i camera
lsusb | grep -i fingerprint
```

---

## Navigation

**Previous:** **[← Phase 07: Security](SECURITY.md)**

**Next:** **[Phase 09: Finalize →](FINALIZE.md)**

---

**Back to:** [Main Index](../INDEX.md) | [Repository Root](../README.md)

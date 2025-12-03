# Phase 05: Network Configuration

**Purpose:** Configure network connectivity (NetworkManager, WiFi, Bluetooth). Establishing a reliable network connection is fundamental for system updates, software installation, and internet access. For a comprehensive guide on network setup in Arch Linux, refer to the [ArchWiki on Network configuration](https://wiki.archlinux.org/title/Network_configuration).

**Prerequisites:**
- Inside chroot environment OR after first boot

**Time Estimate:** 5-10 minutes
**Difficulty:** ⭐ Easy

**Steps (execute in order):**

1. **[NetworkManager](../steps/networkmanager.md)**
   - Enable NetworkManager service

2. **[WiFi Support](../steps/wifi.md)** (if using WiFi)
   - Install WiFi packages
   - Configure after first boot

3. **[Bluetooth](../steps/bluetooth.md)** (if using Bluetooth)
   - Install Bluetooth packages
   - Enable Bluetooth service

## Verification

```bash
systemctl is-enabled NetworkManager
systemctl is-enabled bluetooth
```

---

## Navigation

**Previous:** **[← Phase 04: Bootloader](BOOTLOADER.md)**

**Next:**
- **[Phase 06: Audio →](AUDIO.md)** (optional - if you need sound)
- **[Phase 07: Security →](SECURITY.md)** (optional - recommended)
- **[Phase 09: Finalize →](FINALIZE.md)** (if skipping audio/security)

---

**Back to:** [Main Index](../INDEX.md) | [Repository Root](../README.md)

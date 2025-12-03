# Phase 05: Network Configuration

**Purpose:** Configure network connectivity (NetworkManager, WiFi, Bluetooth). Establishing a reliable network connection is fundamental for system updates, software installation, and internet access. For a comprehensive guide on network setup in Arch Linux, refer to the [ArchWiki on Network configuration](https://wiki.archlinux.org/title/Network_configuration).

**Prerequisites:**
- Inside chroot environment OR after first boot

**Time Estimate:** 5-10 minutes
**Difficulty:** ⭐ Easy

**Steps (execute in order):**

1. **[NetworkManager](../steps/networkmanager-commands.md)**
   - Enable NetworkManager service

2. **[WiFi Support](../steps/wifi-commands.md)** (if using WiFi)
   - Install WiFi packages
   - Configure after first boot

3. **[Bluetooth](../steps/bluetooth-commands.md)** (if using Bluetooth)
   - Install Bluetooth packages
   - Enable Bluetooth service

## Verification

```bash
systemctl is-enabled NetworkManager
systemctl is-enabled bluetooth
```

---

## Navigation

**Previous:** **[← Phase 04: Bootloader](04-bootloader.md)**

**Next:**
- **[Phase 06: Audio →](06-audio.md)** (optional - if you need sound)
- **[Phase 07: Security →](07-security.md)** (optional - recommended)
- **[Phase 09: Finalize →](09-finalize.md)** (if skipping audio/security)

---

**Back to:** [Main Index](../index.md) | [Repository Root](../README.md)

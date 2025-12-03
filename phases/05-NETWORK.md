# Phase 05: Network Configuration

**Purpose:** Configure network connectivity (NetworkManager, WiFi, Bluetooth)

**Prerequisites:**
- Inside chroot environment OR after first boot

**Steps (execute in order):**

1. **[NetworkManager](../steps/11-networkmanager.md)**
   - Enable NetworkManager service

2. **[WiFi Support](../steps/12-wifi.md)** (if using WiFi)
   - Install WiFi packages
   - Configure after first boot

3. **[Bluetooth](../steps/13-bluetooth.md)** (if using Bluetooth)
   - Install Bluetooth packages
   - Enable Bluetooth service

## Verification

```bash
systemctl is-enabled NetworkManager
systemctl is-enabled bluetooth
```

## Next Phase

**[Phase 06: Audio](06-AUDIO.md)** (optional) or **[Phase 07: Security](07-SECURITY.md)** (optional) or **[Phase 09: Finalize](09-FINALIZE.md)**

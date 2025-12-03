# Phase 06: Audio Configuration

**Purpose:** Configure audio server (PipeWire). Setting up an audio server is crucial for sound output and input, enabling multimedia, communication, and various applications. For comprehensive details on audio in Arch Linux, especially with PipeWire, refer to the [ArchWiki on PipeWire](https://wiki.archlinux.org/title/PipeWire).

**Prerequisites:**
- Inside chroot environment OR after first boot

**Time Estimate:** 5-10 minutes
**Difficulty:** ⭐ Easy

**Steps:**

1. **[Audio Server Setup](../steps/audio-commands.md)**
   - Install PipeWire audio stack
   - Enable PipeWire services

## Verification

```bash
systemctl --user is-enabled pipewire
systemctl --user is-enabled pipewire-pulse
```

---

## Navigation

**Previous:** **[← Phase 05: Network](05-network.md)**

**Next:**
- **[Phase 07: Security →](07-security.md)** (optional - recommended)
- **[Phase 09: Finalize →](09-finalize.md)** (if skipping security)

---

**Back to:** [Main Index](../index.md) | [Repository Root](../README.md)

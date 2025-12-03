# Phase 06: Audio Configuration

**Purpose:** Configure audio server (PipeWire). Setting up an audio server is crucial for sound output and input, enabling multimedia, communication, and various applications. For comprehensive details on audio in Arch Linux, especially with PipeWire, refer to the [ArchWiki on PipeWire](https://wiki.archlinux.org/title/PipeWire).

**Prerequisites:**
- Inside chroot environment OR after first boot

**Steps:**

1. **[Audio Server Setup](../steps/14-audio.md)**
   - Install PipeWire audio stack
   - Enable PipeWire services

## Verification

```bash
systemctl --user is-enabled pipewire
systemctl --user is-enabled pipewire-pulse
```

## Next Phase

**[Phase 07: Security](07-SECURITY.md)** (optional) or **[Phase 09: Finalize](09-FINALIZE.md)**

# Module: Audio Server Setup

**Purpose:** Install and configure audio server (PipeWire)

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)

**Time:** 3-5 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## Step 1: Install PipeWire Audio Stack

```bash
pacman -S \
  pipewire \
  pipewire-alsa \
  pipewire-pulse \
  pipewire-jack \
  wireplumber \
  pavucontrol
```

**Package breakdown:**
- `pipewire` - Modern audio server
- `pipewire-alsa` - ALSA compatibility
- `pipewire-pulse` - PulseAudio compatibility
- `pipewire-jack` - JACK compatibility
- `wireplumber` - Session manager
- `pavucontrol` - Audio control GUI

---

## Step 2: Enable PipeWire Services

```bash
systemctl --user enable pipewire pipewire-pulse wireplumber
```

**Note:** User services are enabled per-user, not system-wide

---

**SUCCESS:** Audio server installed

**After reboot and login:**
- Audio should work automatically
- Use `pavucontrol` to control audio settings
- PipeWire provides compatibility with ALSA, PulseAudio, and JACK applications

---

**Next:** Continue with other configuration modules

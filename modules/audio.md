# Module: Audio Server Setup

**Purpose:** Install and configure audio server (PipeWire). [PipeWire](https://wiki.archlinux.org/title/PipeWire) is a server for handling audio, video streams, and hardware on Linux. It aims to be a modern replacement for PulseAudio and JACK, offering lower latency and improved functionality.

**Prerequisites:**
- Inside chroot environment (module `chroot.md`)

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
- `pipewire`: The core PipeWire server and client libraries. [ArchWiki: PipeWire](https://wiki.archlinux.org/title/PipeWire).
- `pipewire-alsa`: ALSA (Advanced Linux Sound Architecture) compatibility for PipeWire.
- `pipewire-pulse`: PulseAudio compatibility for PipeWire, allowing applications designed for PulseAudio to use PipeWire. [ArchWiki: PulseAudio](https://wiki.archlinux.org/title/PulseAudio).
- `pipewire-jack`: JACK Audio Connection Kit compatibility for PipeWire, crucial for professional audio applications. [ArchWiki: JACK Audio Connection Kit](https://wiki.archlinux.org/title/JACK_Audio_Connection_Kit).
- `wireplumber`: A session manager for PipeWire, handling policy and session management.
- `pavucontrol`: A GTK-based volume control application for PulseAudio (and thus PipeWire via `pipewire-pulse`). [ArchWiki: PulseAudio/Examples#pavucontrol](https://wiki.archlinux.org/title/PulseAudio/Examples#pavucontrol).

---

## Step 2: Enable PipeWire Services

PipeWire services are typically enabled as user services, meaning they run in the user's session rather than as system-wide daemons. This approach offers better security and flexibility. For more on user services with `systemd`, refer to [ArchWiki: systemd/User#User_services](https://wiki.archlinux.org/title/Systemd/User#User_services).

```bash
systemctl --user enable pipewire pipewire-pulse wireplumber
```

**Note:** User services are enabled per-user, not system-wide

---

**SUCCESS:** Audio server installed

**After reboot and login:**
- Audio should work automatically
- Use `pavucontrol` to control audio settings. `pavucontrol` (PulseAudio Volume Control) is a graphical tool often used with PipeWire via its PulseAudio compatibility layer. See [ArchWiki: PulseAudio/Examples#pavucontrol](https://wiki.archlinux.org/title/PulseAudio/Examples#pavucontrol).
- PipeWire provides compatibility with ALSA, PulseAudio, and JACK applications

---

## Troubleshooting

For more extensive troubleshooting on PipeWire, refer to the [ArchWiki on PipeWire#Troubleshooting](https://wiki.archlinux.org/title/PipeWire#Troubleshooting).

### Problem: No sound after installation
**Solution:**
1. Check PipeWire service: `systemctl --user status pipewire`
2. Check PulseAudio compatibility: `systemctl --user status pipewire-pulse`
3. Restart services: `systemctl --user restart pipewire pipewire-pulse`
4. Check audio devices: `pactl list short sinks`

### Problem: PipeWire service fails to start
**Solution:**
1. Check user services: `systemctl --user status pipewire`
2. Verify installation: `pacman -Q pipewire pipewire-pulse pipewire-alsa`
3. Check logs: `journalctl --user -u pipewire`
4. Reinstall if needed: `pacman -S pipewire pipewire-pulse pipewire-alsa`

### Problem: Applications can't find audio
**Solution:**
1. Ensure pipewire-pulse is running: `systemctl --user status pipewire-pulse`
2. Set environment variables: `export PULSE_RUNTIME_PATH=/run/user/$(id -u)/pulse`
3. Restart applications
4. Check ALSA: `aplay -l` should list devices

---

**Next:** Continue with other configuration modules

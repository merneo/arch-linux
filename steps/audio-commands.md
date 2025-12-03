# Step: Audio Server Setup

**Purpose:** Install and configure audio server (PipeWire). [PipeWire](https://wiki.archlinux.org/title/PipeWire) is a server for handling audio, video streams, and hardware on Linux.

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment

## Commands

```bash
# Install PipeWire audio stack. See [ArchWiki: PipeWire#Installation](https://wiki.archlinux.org/title/PipeWire#Installation).
pacman -S pipewire pipewire-alsa pipewire-pulse pipewire-jack wireplumber pavucontrol

# Enable PipeWire services (user services). See [ArchWiki: systemd/User#User services](https://wiki.archlinux.org/title/Systemd/User#User_services).
systemctl --user enable pipewire pipewire-pulse wireplumber
```

**NEXT:** Continue with other modules or `exit-chroot.md`

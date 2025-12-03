# Step: Audio Server Setup

**ENVIRONMENT:** Chroot (root@archiso /)#
**PREREQUISITES:** Inside chroot environment

## Commands

```bash
# Install PipeWire audio stack
pacman -S pipewire pipewire-alsa pipewire-pulse pipewire-jack wireplumber pavucontrol

# Enable PipeWire services (user services)
systemctl --user enable pipewire pipewire-pulse wireplumber
```

**NEXT:** Continue with other modules or `15-exit-chroot.md`

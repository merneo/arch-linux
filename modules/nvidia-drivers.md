# Module: NVIDIA GPU Driver Installation

**Purpose:** Install proprietary NVIDIA drivers for optimal performance on systems with NVIDIA graphics cards. The [ArchWiki on NVIDIA](https://wiki.archlinux.org/title/NVIDIA) provides extensive documentation on driver installation, configuration, and troubleshooting for NVIDIA GPUs.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `user-creation.md`)
- Network connectivity (see `networkmanager.md`)
- Xorg Display Server installed (see `xorg-config.md`) or a Wayland compositor with NVIDIA EGLStreams support.
- Kernel headers installed (should be present if `linux-headers` was installed during core installation).

**Time:** 10-20 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Blacklist Nouveau (Open-Source Driver)

The open-source `nouveau` driver, which is part of the Linux kernel, can conflict with the proprietary NVIDIA drivers. It is crucial to blacklist it to ensure the NVIDIA drivers load correctly. For more details on this process, refer to the [ArchWiki on Nouveau](https://wiki.archlinux.org/title/Nouveau) and [ArchWiki on mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio).

```bash
sudo tee /etc/modprobe.d/blacklist-nouveau.conf << EOF
blacklist nouveau
options nouveau modeset=0
EOF
```

After blacklisting, you need to regenerate your initramfs.

```bash
sudo mkinitcpio -P
```

---

## Step 2: Install NVIDIA Drivers

Installing the official NVIDIA proprietary drivers is essential for unlocking the full performance and features of your NVIDIA graphics card.

```bash
sudo pacman -S nvidia nvidia-utils nvidia-settings
```

**Package breakdown:**
- `nvidia`: The core [NVIDIA graphics driver](https://wiki.archlinux.org/title/NVIDIA).
- `nvidia-utils`: Essential NVIDIA utilities, including `nvidia-smi` for monitoring GPU usage.
- `nvidia-settings`: A graphical tool for configuring various NVIDIA display settings.

**Note on Kernel Versions:**
- If you are running a `linux-lts` kernel, you might need to install `nvidia-lts` instead of `nvidia`. See [ArchWiki: NVIDIA#Installation](https://wiki.archlinux.org/title/NVIDIA#Installation).
- If you use a custom or other kernel, ensure you install the corresponding `nvidia-dkms` package. [DKMS (Dynamic Kernel Module Support)](https://wiki.archlinux.org/title/Dynamic_Kernel_Module_Support) is crucial as it automatically rebuilds kernel modules when new kernel versions are installed.

---

## Step 3: Configure Xorg (Optional, but Recommended)

Explicitly configuring Xorg to use the NVIDIA driver can prevent issues and ensure optimal performance. The `nvidia-xconfig` utility helps generate a suitable `xorg.conf` file. For more details on Xorg configuration, refer to the [ArchWiki on Xorg#Configuration](https://wiki.archlinux.org/title/Xorg#Configuration) and [ArchWiki: NVIDIA#Xorg_configuration](https://wiki.archlinux.org/title/NVIDIA#Xorg_configuration).

```bash
sudo nvidia-xconfig
```

This will generate an `/etc/X11/xorg.conf` file. If you already have a custom `xorg.conf` or snippets in `xorg.conf.d`, merge these changes carefully.
---

## Step 4: Reboot

Reboot your system to ensure the Nouveau driver is blacklisted and the NVIDIA drivers are loaded correctly.

```bash
reboot
```

---

## Step 5: Verify Installation

After reboot and logging into your desktop environment, you can verify the drivers are working.

```bash
nvidia-smi
```
This command, part of [NVIDIA's System Management Interface](https://developer.nvidia.com/nvidia-smi), should display information about your NVIDIA GPU (e.g., driver version, GPU temperature, memory usage).

You can also open `nvidia-settings` (usually from your application launcher) to further configure your display. For detailed usage, consult the [NVIDIA X Server Settings documentation](https://us.download.nvidia.com/XFree86/Linux-x86_64/latest/README/xserver-settings.html).

---

**SUCCESS:** NVIDIA proprietary drivers installed and configured.

**Next:** Optimize your display settings with `nvidia-settings` or proceed with other post-installation tasks.

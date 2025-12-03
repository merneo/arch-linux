# Module: AMD GPU Driver Installation

**Purpose:** Ensure proper setup for AMD graphics cards, typically using the open-source `amdgpu` driver which is included in the Linux kernel. This module focuses on verifying setup and installing optional utilities for optimal performance and compatibility. For comprehensive information, refer to the [ArchWiki on AMDGPU](https://wiki.archlinux.org/title/AMDGPU).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `04-user-creation.md`)
- Network connectivity (see `11-networkmanager.md`)
- Xorg Display Server installed (see `27-xorg-config.md`) or a Wayland compositor.
- Kernel headers installed (should be present if `linux-headers` was installed during core installation).

**Time:** 5-10 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Verify `amdgpu` Driver

The `amdgpu` open-source driver is typically included and loaded automatically by the Linux kernel for most modern AMD GPUs. You can use `lspci` to list PCI devices and check which kernel driver is in use. For details, refer to the [ArchWiki on lspci](https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF#Verifying_VT-d_is_enabled) (specifically the `lspci -k` usage for kernel modules).

You can verify if the driver is loaded:

```bash
lspci -k | grep -EA3 'VGA|3D|Display'
```

Look for a line similar to `Kernel driver in use: amdgpu`. If you see this, the driver is likely working.

---

## Step 2: Install Vulkan Drivers and Other Utilities

For optimal performance and compatibility with modern games and applications, especially those requiring 3D acceleration, installing Vulkan drivers and multimedia utilities is crucial. For a comprehensive overview of [Vulkan](https://wiki.archlinux.org/title/Vulkan) in Arch Linux, refer to its ArchWiki page.

```bash
sudo pacman -S amdvlk vulkan-radeon libva-mesa-driver mesa-vdpau mesa-opencl opencl-amd
```

**Package breakdown:**
- `amdvlk`: AMD's official [Vulkan driver](https://wiki.archlinux.org/title/Vulkan) (open-source implementation).
- `vulkan-radeon`: The [Radeon Vulkan driver](https://wiki.archlinux.org/title/Vulkan) from the Mesa project, often preferred for gaming performance on AMD GPUs.
- `libva-mesa-driver`: Provides [VA-API (Video Acceleration API)](https://wiki.archlinux.org/title/Hardware_video_acceleration) support for Mesa drivers, enabling hardware-accelerated video decoding.
- `mesa-vdpau`: Provides [VDPAU (Video Decode and Presentation API for Unix)](https://wiki.archlinux.org/title/Hardware_video_acceleration) support for Mesa drivers, another hardware video decoding API.
- `mesa-opencl`: OpenCL (Open Computing Language) support provided by [Mesa](https://wiki.archlinux.org/title/Mesa).
- `opencl-amd`: AMD's official OpenCL driver for [ROCm](https://wiki.archlinux.org/title/GPGPU#ROCm)-compatible GPUs.

---

## Step 3: Install `xorg-xrandr` (for Xorg)

[XRandr](https://wiki.archlinux.org/title/Xrandr) is an X Window System extension that allows dynamic configuration of output displays. It is a useful utility for setting screen resolution, refresh rate, and managing multiple monitors in an Xorg environment.

If you are using Xorg, `xorg-xrandr` is a useful utility for display configuration.

```bash
sudo pacman -S xorg-xrandr
```

---

## Step 4: Configure Xorg (Optional)

In most cases, with modern AMD GPUs and the `amdgpu` driver, explicit Xorg configuration via `/etc/X11/xorg.conf.d/` files is not strictly required as the driver handles much of the setup automatically. However, specific scenarios or troubleshooting might necessitate a custom configuration. For more details, refer to the [ArchWiki on Xorg#Configuration](https://wiki.archlinux.org/title/Xorg#Configuration).

Example `/etc/X11/xorg.conf.d/20-amdgpu.conf`:
```
Section "OutputClass"
    Identifier "AMDgpu"
    MatchDriver "amdgpu"
    Driver "amdgpu"
EndSection
```
This is generally not needed.

---

## Step 5: Reboot (if needed)

If you installed new drivers or utilities, a reboot might be beneficial to ensure everything is loaded correctly, especially if you had issues with the `amdgpu` driver initially.

```bash
reboot
```

---

## Step 6: Verify Installation

After reboot and logging into your desktop environment, you can verify that the AMDGPU drivers and graphics APIs are correctly installed and functioning.

```bash
glxinfo -B # Requires 'mesa-utils' to be installed. For more on OpenGL and glxinfo, refer to the [ArchWiki on OpenGL](https://wiki.archlinux.org/title/OpenGL).
```
This command should show `OpenGL renderer string: AMD ...` and indicate direct rendering.

You can also check Vulkan support:
```bash
vulkaninfo # Requires 'vulkan-tools' to be installed. For more on Vulkan, refer to the [ArchWiki on Vulkan](https://wiki.archlinux.org/title/Vulkan).
```

---

**SUCCESS:** AMD GPU drivers and utilities installed and configured.

**Next:** Optimize your display settings or proceed with other post-installation tasks.

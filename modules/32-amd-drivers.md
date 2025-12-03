# Module: AMD GPU Driver Installation

**Purpose:** Ensure proper setup for AMD graphics cards, typically using the open-source `amdgpu` driver which is included in the Linux kernel. This module focuses on verifying setup and installing optional utilities.

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

The `amdgpu` open-source driver is typically included and loaded automatically by the Linux kernel for most modern AMD GPUs.

You can verify if the driver is loaded:

```bash
lspci -k | grep -EA3 'VGA|3D|Display'
```

Look for a line similar to `Kernel driver in use: amdgpu`. If you see this, the driver is likely working.

---

## Step 2: Install Vulkan Drivers and Other Utilities

For optimal performance and compatibility with modern games and applications, install the Vulkan drivers and a few essential utilities.

```bash
sudo pacman -S amdvlk vulkan-radeon libva-mesa-driver mesa-vdpau mesa-opencl opencl-amd
```

**Package breakdown:**
- `amdvlk`: AMD's official Vulkan driver (open-source).
- `vulkan-radeon`: Radeon Vulkan driver (part of Mesa, also open-source). You can choose one or both. `vulkan-radeon` is often preferred for gaming performance.
- `libva-mesa-driver`: VA-API (Video Acceleration API) for Mesa, enables hardware video decoding.
- `mesa-vdpau`: VDPAU (Video Decode and Presentation API for Unix) for Mesa, another hardware video decoding API.
- `mesa-opencl`: OpenCL support for Mesa drivers.
- `opencl-amd`: AMD's official OpenCL driver for ROCm-compatible GPUs.

---

## Step 3: Install `xorg-xrandr` (for Xorg)

If you are using Xorg, `xorg-xrandr` is a useful utility for display configuration.

```bash
sudo pacman -S xorg-xrandr
```

---

## Step 4: Configure Xorg (Optional)

In most cases, with modern AMD GPUs and the `amdgpu` driver, explicit Xorg configuration is not required. However, if you encounter issues, you might need a minimal `xorg.conf` file.

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

After reboot and logging into your desktop environment, you can verify:

```bash
glxinfo -B # Requires 'mesa-utils' to be installed.
```
This command should show `OpenGL renderer string: AMD ...` and indicate direct rendering.

You can also check Vulkan support:
```bash
vulkaninfo # Requires 'vulkan-tools' to be installed.
```

---

**SUCCESS:** AMD GPU drivers and utilities installed and configured.

**Next:** Optimize your display settings or proceed with other post-installation tasks.

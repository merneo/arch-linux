# Module: AMD Graphics Drivers

**Purpose:** Install AMD graphics drivers (Mesa, Vulkan, VA-API)

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)

**Time:** 3-5 minutes

---

## Step 1: Install AMD Graphics Drivers

```bash
# Install AMD graphics drivers
pacman -S mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon \
  xf86-video-amdgpu
```

**Package breakdown:**
- `mesa` - OpenGL implementation
- `lib32-mesa` - 32-bit OpenGL (for compatibility)
- `vulkan-radeon` - Vulkan driver for AMD Radeon
- `lib32-vulkan-radeon` - 32-bit Vulkan
- `xf86-video-amdgpu` - X11 AMD driver

---

## Step 2: Verify Installation

```bash
# Check installed packages
pacman -Q | grep -E "mesa|vulkan|amdgpu|radeon"

# Should show:
# mesa ...
# lib32-mesa ...
# vulkan-radeon ...
# lib32-vulkan-radeon ...
# xf86-video-amdgpu ...
```

---

**SUCCESS:** AMD graphics drivers installed

**Official Resources:**
- [AMD Graphics on ArchWiki](https://wiki.archlinux.org/title/AMDGPU)
- [Mesa 3D Graphics Library](https://www.mesa3d.org/)
- [AMDGPU Driver](https://www.kernel.org/doc/html/latest/gpu/amdgpu.html)

**Next:** Continue with other configuration modules

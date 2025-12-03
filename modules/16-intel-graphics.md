# Module: Intel Graphics Drivers

**Purpose:** Install Intel graphics drivers (Mesa, Vulkan, VA-API)

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)

**Time:** 3-5 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## Step 1: Install Intel Graphics Drivers

```bash
# Install Intel graphics drivers
pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel \
  xf86-video-intel intel-media-driver
```

**Package breakdown:**
- `mesa` - OpenGL implementation
- `lib32-mesa` - 32-bit OpenGL (for compatibility)
- `vulkan-intel` - Vulkan driver for Intel
- `lib32-vulkan-intel` - 32-bit Vulkan
- `xf86-video-intel intel-media-driver` - X11 AMD driver

---

## Step 2: Verify Installation

```bash
# Check installed packages
pacman -Q | grep -E "mesa|vulkan|intel"

# Should show:
# mesa ...
# lib32-mesa ...
# vulkan-intel ...
# lib32-vulkan-intel ...
# xf86-video-intel intel-media-driver ...
```

---

**SUCCESS:** Intel graphics drivers installed

**Official Resources:**
- [Intel Graphics on ArchWiki](https://wiki.archlinux.org/title/Intel graphics)
- [Mesa 3D Graphics Library](https://www.mesa3d.org/)
- [Intel graphics Driver](https://github.com/intel/media-driver)

**Next:** Continue with other configuration modules

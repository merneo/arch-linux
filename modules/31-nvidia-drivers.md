# Module: NVIDIA GPU Driver Installation

**Purpose:** Install proprietary NVIDIA drivers for optimal performance on systems with NVIDIA graphics cards.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `04-user-creation.md`)
- Network connectivity (see `11-networkmanager.md`)
- Xorg Display Server installed (see `27-xorg-config.md`) or a Wayland compositor with NVIDIA EGLStreams support.
- Kernel headers installed (should be present if `linux-headers` was installed during core installation).

**Time:** 10-20 minutes

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Blacklist Nouveau (Open-Source Driver)

The open-source `nouveau` driver can conflict with the proprietary NVIDIA drivers. It's best to blacklist it.

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

Install the main NVIDIA driver package and utilities.

```bash
sudo pacman -S nvidia nvidia-utils nvidia-settings
```

**Package breakdown:**
- `nvidia`: The core NVIDIA graphics driver.
- `nvidia-utils`: Essential NVIDIA utilities.
- `nvidia-settings`: GUI tool for configuring NVIDIA driver settings.

**Note on Kernel Versions:**
- If you are running a `linux-lts` kernel, you might need to install `nvidia-lts` instead of `nvidia`.
- If you use a custom or other kernel, ensure you install the corresponding `nvidia-dkms` package and have `dkms` installed.

---

## Step 3: Configure Xorg (Optional, but Recommended)

It's often good practice to explicitly tell Xorg to use the NVIDIA driver.

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
This command should display information about your NVIDIA GPU.

You can also open `nvidia-settings` (usually from your application launcher) to further configure your display.

---

**SUCCESS:** NVIDIA proprietary drivers installed and configured.

**Next:** Optimize your display settings with `nvidia-settings` or proceed with other post-installation tasks.

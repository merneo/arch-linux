# Module: Essential Applications

**Purpose:** Install a set of common and essential applications for daily use. This module provides a foundation for productivity, communication, and basic system interaction. For a broader selection of software available in Arch Linux, refer to the [ArchWiki on Applications](https://wiki.archlinux.org/title/List_of_applications).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `user-creation.md`)
- Network connectivity (see `networkmanager.md`)
- (Optional) Desktop environment installed (e.g., GNOME, KDE, XFCE)

**Time:** 10-20 minutes (depending on selected applications and internet speed)

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Install a Web Browser

A web browser is fundamental for internet access and is often one of the first applications users install. Here are several popular options:

### Option A: Firefox (Open-Source, Official Repository)
[Firefox](https://wiki.archlinux.org/title/Firefox) is a popular open-source choice, known for its strong privacy features, extensive customization, and is directly available in the official Arch Linux repositories.

```bash
sudo pacman -S firefox
```

### Option B: Google Chrome (Proprietary, AUR)
[Google Chrome](https://wiki.archlinux.org/title/Google_Chrome) is a widely used proprietary browser known for its performance and extensive ecosystem of extensions. It is available via the Arch User Repository (AUR) and requires an [AUR helper](https://wiki.archlinux.org/title/AUR_helpers) like `yay`.

```bash
yay -S google-chrome
```

### Option C: Brave Browser (Open-Source, AUR)
[Brave Browser](https://brave.com/linux/) is an open-source browser focused on privacy, blocking ads and trackers by default, and integrating a cryptocurrency-based reward system. It is available via the AUR.

```bash
yay -S brave-bin
```

### Option D: Opera Browser (Proprietary, Official Repository via `opera-beta`)
[Opera Browser](https://www.opera.com/browsers/opera-for-linux) is a proprietary browser known for its unique features like a built-in VPN, ad blocker, and workspace management. It can be installed from the Arch User Repository (AUR) or sometimes directly from a custom repository. For consistency, we'll suggest the AUR package `opera-beta` or `opera`.

```bash
yay -S opera
```

---

## Step 2: Install a Text Editor / IDE

Choosing the right text editor or Integrated Development Environment (IDE) is a personal preference that significantly impacts productivity.

**Option A: Visual Studio Code (via AUR helper)**
[Visual Studio Code](https://wiki.archlinux.org/title/Visual_Studio_Code) is a highly popular and feature-rich code editor with extensive language support and extensions. It's available in the AUR, meaning you'll need an AUR helper (like `yay`) installed.

```bash
yay -S visual-studio-code-bin
```

**Option B: Neovim (Terminal-based text editor)**
[Neovim](https://wiki.archlinux.org/title/Neovim) is a modern, extensible Vim-based text editor that runs in the terminal, favored by many developers for its speed and powerful customization.

```bash
sudo pacman -S neovim
```

**Option C: Nano (Simple terminal-based text editor)**
[Nano](https://wiki.archlinux.org/title/Nano) is a very simple and user-friendly terminal-based text editor, often a good choice for quick edits or beginners. It's often already installed as part of the base system.

```bash
sudo pacman -S nano
```

---

## Step 3: Install an Office Suite

An office suite is essential for document creation, spreadsheets, and presentations. [LibreOffice](https://wiki.archlinux.org/title/LibreOffice) is a free and open-source office suite, a powerful and popular alternative to proprietary software.

```bash
sudo pacman -S libreoffice-still
```

**Note:** `libreoffice-fresh` is also available for the latest features, while `libreoffice-still` offers greater stability.

---

## Step 4: Install a Terminal Emulator (if not part of DE)

A terminal emulator provides a text-based interface to the operating system, essential for command-line operations. While most desktop environments include one, standalone options like Alacritty offer high performance. For more options, see [ArchWiki: Terminal emulators](https://wiki.archlinux.org/title/Terminal_emulators).

If you're running a minimal setup without a full desktop environment, you might need a dedicated terminal emulator. If using a DE, it likely comes with one (e.g., GNOME Terminal, Konsole, Xfce Terminal).

```bash
sudo pacman -S alacritty # or kitty, terminator, etc. [ArchWiki: Alacritty](https://wiki.archlinux.org/title/Alacritty)
```

---

## Step 5: Install a File Manager (if not part of DE)

A file manager provides a graphical interface for navigating and managing files and directories. Like terminal emulators, most desktop environments include one. For more options, see [ArchWiki: File managers](https://wiki.archlinux.org/title/File_manager).

Similar to terminal emulators, if you're not using a full DE, you might want a standalone file manager.

```bash
sudo pacman -S thunar # or pcmanfm, nemo, etc. [ArchWiki: Thunar](https://wiki.archlinux.org/title/Thunar)
```

---

## Step 6: Install an Archiving Tool

Tools for creating and extracting archives are essential for handling compressed files. For more information on compression utilities, refer to the [ArchWiki on Archiving and compression](https://wiki.archlinux.org/title/Archiving_and_compression).

```bash
sudo pacman -S zip unzip p7zip unrar
```

---

**SUCCESS:** Essential applications installed.

---

## Troubleshooting

For more extensive troubleshooting on package management, refer to the [ArchWiki on Pacman#Troubleshooting](https://wiki.archlinux.org/title/Pacman#Troubleshooting).

### Problem: Package installation fails
**Solution:**
1. Update package database: `sudo pacman -Sy`
2. Check internet connection: `ping archlinux.org`
3. Clear pacman cache: `sudo pacman -Sc`
4. Retry installation

### Problem: AUR helper (yay) not working
**Solution:**
1. Verify yay is installed: `which yay`
2. Check AUR access: `ping aur.archlinux.org`
3. Update yay: `yay -Syu`
4. Reinstall if needed: `yay -S yay`

### Problem: Application doesn't start
**Solution:**
1. Check if installed: `pacman -Q <package>` or `yay -Q <package>`
2. Verify executable exists: `which <application>`
3. Check dependencies: `ldd $(which <application>)`
4. Check logs: `journalctl -xe` for systemd errors

---

**Next:** Explore other software from the official repositories or the AUR to further customize your system.

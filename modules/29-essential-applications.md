# Module: Essential Applications

**Purpose:** Install a set of common and essential applications for daily use.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `04-user-creation.md`)
- Network connectivity (see `11-networkmanager.md`)
- (Optional) Desktop environment installed (e.g., GNOME, KDE, XFCE)

**Time:** 10-20 minutes (depending on selected applications and internet speed)

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Install a Web Browser

A web browser is fundamental for internet access. Firefox is a popular open-source choice.

```bash
sudo pacman -S firefox
```

---

## Step 2: Install a Text Editor / IDE

Choose a text editor or Integrated Development Environment (IDE) based on your preference.

**Option A: Visual Studio Code (via AUR helper)**
VS Code is a popular choice for development, but it's available in the AUR. You'll need an AUR helper (like `yay`) installed.

```bash
yay -S visual-studio-code-bin
```

**Option B: Neovim (Terminal-based text editor)**
Neovim is a powerful and highly configurable terminal-based text editor.

```bash
sudo pacman -S neovim
```

**Option C: Nano (Simple terminal-based text editor)**
Nano is a very simple and user-friendly terminal-based text editor. It's often already installed.

```bash
sudo pacman -S nano
```

---

## Step 3: Install an Office Suite

LibreOffice is a free and open-source office suite, a popular alternative to Microsoft Office.

```bash
sudo pacman -S libreoffice-still
```

**Note:** `libreoffice-fresh` is also available for the latest features, while `libreoffice-still` offers greater stability.

---

## Step 4: Install a Terminal Emulator (if not part of DE)

If you're running a minimal setup without a full desktop environment, you might need a dedicated terminal emulator. If using a DE, it likely comes with one (e.g., GNOME Terminal, Konsole, Xfce Terminal).

```bash
sudo pacman -S alacritty # or kitty, terminator, etc.
```

---

## Step 5: Install a File Manager (if not part of DE)

Similar to terminal emulators, if you're not using a full DE, you might want a standalone file manager.

```bash
sudo pacman -S thunar # or pcmanfm, nemo, etc.
```

---

## Step 6: Install an Archiving Tool

```bash
sudo pacman -S zip unzip p7zip unrar
```

---

**SUCCESS:** Essential applications installed.

**Next:** Explore other software from the official repositories or the AUR to further customize your system.

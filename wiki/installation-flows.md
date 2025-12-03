# Arch Linux Installation Flows: Your Personalized Guide

**Purpose:** This document helps you navigate the modular Arch Linux installation system by providing structured flows and decision points for common installation scenarios. Choose the path that best suits your needs! For a comprehensive overview of the Arch Linux installation process, refer to the [ArchWiki Installation Guide](https://wiki.archlinux.org/title/Installation_guide).

---

## How to Use This Guide

Start with your primary installation goal (e.g., "Clean Install," "Dual Boot"). Follow the questions and choices provided to build your customized installation plan. Each step links to a dedicated module or phase for detailed instructions.

---

## 🚀 Start Here: Choose Your Installation Scenario

### 1. **Clean Installation (Single Boot Arch Linux)**

*   **Do you want disk encryption (LUKS)?**
    *   **Yes, with LUKS:** Proceed to **[Full Installation with Encryption](installation-flows.md#full-installation-with-encryption)**
    *   **No encryption:** Proceed to **[Standard Installation without Encryption](installation-flows.md#standard-installation-without-encryption)**

### 2. **Dual Boot with Windows**

*   **Have you already installed Windows?**
    *   **No, install Windows first:**
        1.  **[Create Arch Linux USB](../modules/pre-create-arch-usb.md)** (You'll need this for Arch later)
        2.  **[Install Windows](../modules/pre-install-windows.md)** (Ensure you leave free space for Arch Linux)
        3.  Proceed to **[Dual Boot Installation](installation-flows.md#dual-boot-installation)**
    *   **Yes, Windows is already installed:** Proceed to **[Dual Boot Installation](installation-flows.md#dual-boot-installation)**

### 3. **Minimal Server Installation**

*   **Just the core system, no desktop environment, minimal services:** Proceed to **[Minimal Server Installation](installation-flows.md#minimal-server-installation)**

---

## ➡️ Detailed Installation Flows

### Full Installation with Encryption

This flow guides you through a complete Arch Linux installation with LUKS disk encryption (for security) and a Btrfs filesystem (for advanced features like snapshots).

**Recommended for:** Users seeking a robust, modern Arch Linux setup with data protection.

**Phase Overview:**

1.  **[PREPARATION: Create USB & Format Disk](../phases/00-preparation.md)**
    *   Create bootable Arch Linux USB.
    *   (Optional) Format entire disk for a fresh start.
    *   Boot from USB and connect to the network.
2.  **[DISK SETUP: Partitioning, LUKS, Btrfs, Mount](../phases/01-disk-setup.md)**
    *   Partition your disk (EFI, Root, Swap).
    *   Encrypt your Root partition with LUKS.
    *   Create Btrfs filesystem and subvolumes on the encrypted root.
    *   Mount all partitions correctly.
3.  **[SYSTEM INSTALL: Base System & Fstab](../phases/02-system-install.md)**
    *   Install the base Arch Linux system using `pacstrap`.
    *   Generate `fstab`.
4.  **[BASIC CONFIGURATION: Locale, User, Sudo](../phases/03-basic-config.md)**
    *   Set system locale and timezone.
    *   Create your user account and configure `sudo` access.
5.  **[BOOTLOADER: GRUB & Auto-Unlock](../phases/04-bootloader.md)**
    *   Install and configure GRUB bootloader with LUKS auto-unlock.
6.  **[NETWORK: Wired & Wireless Setup](../phases/05-network.md)**
    *   Enable NetworkManager for network connectivity.
    *   Configure WiFi and Bluetooth support.
7.  **[AUDIO: PipeWire Sound Server](../phases/06-audio.md)**
    *   Install and enable PipeWire for audio.
8.  **[SECURITY: Firewall & SSH Protection](../phases/07-security.md)**
    *   Configure UFW firewall.
    *   Install Fail2ban for SSH brute-force protection.
9.  **[Hardware (Optional)](../phases/08-hardware.md)**
    *   Configure touchpad, webcam, IR camera, fingerprint reader if applicable.
**Post-Installation Choices (after rebooting into your new system):**

*   **[AUR Helper (e.g., yay)](post-installation.md#aur-helper-eg-yay)**: Highly recommended for easy access to community packages.
*   **[Display Server Configuration](post-installation.md#display-server-configuration)**: Xorg or Wayland.
*   **[Essential Applications](post-installation.md#essential-applications)**: Install web browser, office suite, etc.
*   **[Backup Solution (Timeshift)](post-installation.md#backup-solutions)**: Set up system snapshots.
*   **[Terminal Tools (w3m)](post-installation.md#w3m-terminal-web-browser)**: For text-based web browsing in console/minimal environments.
*   **[Desktop Environment](post-installation.md#desktop-environments)**: Install GNOME, KDE Plasma, XFCE, i3, Hyprland, or another Window Manager.

---

**Post-Installation Choices (after rebooting into your new system):**

*   **[AUR Helper (e.g., yay)](post-installation.md#aur-helper-eg-yay)**: Highly recommended for easy access to community packages.
*   **[Display Server Configuration](post-installation.md#display-server-configuration)**: Xorg or Wayland.
*   **[Essential Applications](post-installation.md#essential-applications)**: Install web browser, office suite, etc.
*   **[Backup Solution (Timeshift)](post-installation.md#backup-solutions)**: Set up system snapshots.
*   **[Terminal Tools (w3m)](post-installation.md#w3m-terminal-web-browser)**: For text-based web browsing in console/minimal environments.
*   **[Desktop Environment](post-installation.md#desktop-environments)**: Install GNOME, KDE Plasma, XFCE, or a Window Manager.

---

### Standard Installation without Encryption

This flow guides you through a complete Arch Linux installation with a Btrfs filesystem but without disk encryption, providing a simpler setup for users who don't require LUKS.

**Recommended for:** Users who want a modern Arch Linux setup with Btrfs features but prefer a faster boot without encryption.

**Phase Overview:**

1.  **[PREPARATION: Create USB & Format Disk](../phases/00-preparation.md)**
    *   Create bootable Arch Linux USB.
    *   (Optional) Format entire disk for a fresh start.
    *   Boot from USB and connect to the network.
2.  **[DISK SETUP: Partitioning, Btrfs, Mount](../phases/01-disk-setup.md)**
    *   Partition your disk (EFI, Root, Swap).
    *   Create Btrfs filesystem and subvolumes.
    *   Mount all partitions correctly.
3.  **[SYSTEM INSTALL: Base System & Fstab](../phases/02-system-install.md)**
    *   Install the base Arch Linux system using `pacstrap`.
    *   Generate `fstab`.
4.  **[BASIC CONFIGURATION: Locale, User, Sudo](../phases/03-basic-config.md)**
    *   Set system locale and timezone.
    *   Create your user account and configure `sudo` access.
5.  **[BOOTLOADER: GRUB](../phases/04-bootloader.md)**
    *   Install and configure GRUB bootloader (without LUKS parameters).
6.  **[NETWORK: Wired & Wireless Setup](../phases/05-network.md)**
    *   Enable NetworkManager for network connectivity.
    *   Configure WiFi and Bluetooth support.
7.  **[AUDIO: PipeWire Sound Server](../phases/06-audio.md)**
    *   Install and enable PipeWire for audio.
8.  **[SECURITY: Firewall & SSH Protection](../phases/07-security.md)**
    *   Configure UFW firewall.
    *   Install Fail2ban for SSH brute-force protection.
9.  **[Hardware (Optional)](../phases/08-hardware.md)**
    *   Configure touchpad, webcam, IR camera, fingerprint reader if applicable.
**Post-Installation Choices (after rebooting into your new system):**

*   **[AUR Helper (e.g., yay)](post-installation.md#aur-helper-eg-yay)**: Highly recommended for easy access to community packages.
*   **[Display Server Configuration](post-installation.md#display-server-configuration)**: Xorg or Wayland.
*   **[Essential Applications](post-installation.md#essential-applications)**: Install web browser, office suite, etc.
*   **[Backup Solution (Timeshift)](post-installation.md#backup-solutions)**: Set up system snapshots.
*   **[Terminal Tools (w3m)](post-installation.md#w3m-terminal-web-browser)**: For text-based web browsing in console/minimal environments.
*   **[Desktop Environment](post-installation.md#desktop-environments)**: Install GNOME, KDE Plasma, XFCE, i3, Hyprland, or another Window Manager.

---

### Dual Boot Installation

### Dual Boot Installation

This flow focuses on installing Arch Linux alongside an existing Windows installation. For comprehensive guidance on dual-booting, refer to the [ArchWiki on Dual boot with Windows](https://wiki.archlinux.org/title/Dual_boot_with_Windows).

**Recommended for:** Users who want to keep Windows and add Arch Linux to their system.

**Phase Overview:**

1.  **[PREPARATION: Create USB & Prepare Disk](../phases/00-preparation.md)**
    *   Ensure Windows is installed and free space is available for Arch Linux.
    *   Create bootable Arch Linux USB.
    *   Boot from USB and connect to the network.
2.  **[DISK SETUP: Partitioning & Mount](../phases/01-disk-setup.md)**
    *   Partition the free space for Arch Linux (Root, Swap). **Use the existing Windows EFI partition.**
    *   Create filesystems (e.g., Ext4).
    *   Mount all partitions correctly (including Windows EFI).
3.  **[SYSTEM INSTALL: Base System & Fstab](../phases/02-system-install.md)**
    *   Install the base Arch Linux system using `pacstrap`.
    *   Generate `fstab`.
4.  **[BASIC CONFIGURATION: Locale, User, Sudo](../phases/03-basic-config.md)**
    *   Set system locale and timezone.
    *   Create your user account and configure `sudo` access.
5.  **[BOOTLOADER: GRUB & OS Prober](../phases/04-bootloader.md)**
    *   Install and configure GRUB bootloader.
    *   **Crucially, ensure `os-prober` is configured and enabled to detect Windows.**
6.  **[NETWORK: Wired & Wireless Setup](../phases/05-network.md)**
    *   Enable NetworkManager for network connectivity.
    *   Configure WiFi and Bluetooth support.
7.  **[AUDIO: PipeWire Sound Server](../phases/06-audio.md)**
    *   Install and enable PipeWire for audio.
8.  **[SECURITY: Firewall & SSH Protection](../phases/07-security.md)**
    *   Configure UFW firewall.
    *   Install Fail2ban for SSH brute-force protection.
9.  **[FINALIZE: Exit & Reboot](../phases/09-finalize.md)**
    *   Exit chroot, unmount, and reboot into your new system!

**Post-Installation Choices (after rebooting into your new system):**

*   **[AUR Helper (e.g., yay)](post-installation.md#aur-helper-eg-yay)**: Highly recommended for easy access to community packages.
*   **[Display Server Configuration](post-installation.md#display-server-configuration)**: Xorg or Wayland.
*   **[Essential Applications](post-installation.md#essential-applications)**: Install web browser, office suite, etc.
*   **[Backup Solution (Timeshift)](post-installation.md#backup-solutions)**: Set up system snapshots.
*   **[Terminal Tools (w3m)](post-installation.md#w3m-terminal-web-browser)**: For text-based web browsing in console/minimal environments.
*   **[Desktop Environment](post-installation.md#desktop-environments)**: Install GNOME, KDE Plasma, XFCE, i3, Hyprland, or a Window Manager.
*   **[Hardware (Optional)](../phases/08-hardware.md)**: Configure touchpad, webcam, IR camera, fingerprint reader if applicable.

---

### Minimal Server Installation

This flow provides a bare-bones Arch Linux installation, ideal for servers or users who want to build their system from scratch with no graphical environment.

**Recommended for:** Experienced users, servers, or those who prefer a highly customized, minimal setup.

**Phase Overview:**

1.  **[PREPARATION: Create USB & Format Disk](../phases/00-preparation.md)**
    *   Create bootable Arch Linux USB.
    *   (Optional) Format entire disk for a fresh start.
    *   Boot from USB and connect to the network.
2.  **[DISK SETUP: Partitioning & Mount](../phases/01-disk-setup.md)**
    *   Partition your disk (EFI, Root, Swap).
    *   Create filesystems (e.g., Ext4).
    *   Mount all partitions correctly.
3.  **[SYSTEM INSTALL: Base System & Fstab](../phases/02-system-install.md)**
    *   Install the base Arch Linux system using `pacstrap`.
    *   Generate `fstab`.
4.  **[BASIC CONFIGURATION: Locale, User, Sudo](../phases/03-basic-config.md)**
    *   Set system locale and timezone.
    *   Create your user account and configure `sudo` access.
5.  **[BOOTLOADER: GRUB](../phases/04-bootloader.md)**
    *   Install and configure GRUB bootloader.
6.  **[NETWORK: Wired Setup](../phases/05-network.md)**
    *   Enable NetworkManager for network connectivity (focus on wired).
7.  **[SECURITY: Firewall & SSH Server](../phases/07-security.md)**
    *   Configure UFW firewall.
    *   Install and configure SSH server.
    *   Install Fail2ban for SSH brute-force protection.
8.  **[FINALIZE: Exit & Reboot](../phases/09-finalize.md)**
    *   Exit chroot, unmount, and reboot into your new system!

**Post-Installation Choices (after rebooting into your new system):**

*   **[AUR Helper (e.g., yay)](post-installation.md#aur-helper-eg-yay)**: Recommended for easy access to community packages.
*   **[Terminal Tools (w3m)](post-installation.md#w3m-terminal-web-browser)**: For text-based web browsing in console/minimal environments.
*   **[i3 Window Manager](post-installation.md#i3-window-manager)**: As a minimal GUI for server administration.
*   Install any specific server applications or services as needed.

---

## 🛠️ Build Your Own Custom Flow

If none of the above scenarios perfectly match your requirements, you can build your own customized installation procedure:

1.  **Review Available Modules:** Browse the **[Module Index](../module-index.md)** to understand all individual components and their purposes.
2.  **Understand Phases:** Look at the **[Phases](../phases/00-preparation.md)** directory for a logical grouping of steps.
3.  **Consult Steps:** The **[Step Index](../step-index.md)** provides a quick reference for all individual commands.
4.  **Generate a Procedure:** If you have very specific needs, consider how to **[Generate a Custom Procedure](../generate-procedure.md)** using the modular files.

---

**Back to:** [Home](HOME.md) | **Complete Guide:** [complete-installation.md](complete-installation.md)
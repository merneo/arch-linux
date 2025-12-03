# Arch Linux Installation Flows: Your Personalized Guide

**Purpose:** This document helps you navigate the modular Arch Linux installation system by providing structured flows and decision points for common installation scenarios. Choose the path that best suits your needs! For a comprehensive overview of the Arch Linux installation process, refer to the [ArchWiki Installation Guide](https://wiki.archlinux.org/title/Installation_guide).

---

## How to Use This Guide

Start with your primary installation goal (e.g., "Clean Install," "Dual Boot"). Follow the questions and choices provided to build your customized installation plan. Each step links to a dedicated module or phase for detailed instructions.

---

## 🚀 Start Here: Choose Your Installation Scenario

### 1. **Clean Installation (Single Boot Arch Linux)**

*   **Do you want disk encryption (LUKS)?**
    *   **Yes, with LUKS:** Proceed to **[Full Installation with Encryption](#full-installation-with-encryption)**
    *   **No encryption:** Proceed to **[Standard Installation without Encryption](#standard-installation-without-encryption)**

### 2. **Dual Boot with Windows**

*   **Have you already installed Windows?**
    *   **No, install Windows first:**
        1.  **[Create Arch Linux USB](../modules/PRE-01-create-arch-usb.md)** (You'll need this for Arch later)
        2.  **[Install Windows](../modules/PRE-02-install-windows.md)** (Ensure you leave free space for Arch Linux)
        3.  Proceed to **[Dual Boot Installation](#dual-boot-installation)**
    *   **Yes, Windows is already installed:** Proceed to **[Dual Boot Installation](#dual-boot-installation)**

### 3. **Minimal Server Installation**

*   **Just the core system, no desktop environment, minimal services:** Proceed to **[Minimal Server Installation](#minimal-server-installation)**

---

## ➡️ Detailed Installation Flows

### Full Installation with Encryption

This flow guides you through a complete Arch Linux installation with LUKS disk encryption (for security) and a Btrfs filesystem (for advanced features like snapshots).

**Recommended for:** Users seeking a robust, modern Arch Linux setup with data protection.

**Phase Overview:**

1.  **[PREPARATION: Create USB & Format Disk](../phases/PREPARATION.md)**
    *   Create bootable Arch Linux USB.
    *   (Optional) Format entire disk for a fresh start.
    *   Boot from USB and connect to the network.
2.  **[DISK SETUP: Partitioning, LUKS, Btrfs, Mount](../phases/DISK_SETUP.md)**
    *   Partition your disk (EFI, Root, Swap).
    *   Encrypt your Root partition with LUKS.
    *   Create Btrfs filesystem and subvolumes on the encrypted root.
    *   Mount all partitions correctly.
3.  **[SYSTEM INSTALL: Base System & Fstab](../phases/SYSTEM_INSTALL.md)**
    *   Install the base Arch Linux system using `pacstrap`.
    *   Generate `fstab`.
4.  **[BASIC CONFIGURATION: Locale, User, Sudo](../phases/BASIC_CONFIG.md)**
    *   Set system locale and timezone.
    *   Create your user account and configure `sudo` access.
5.  **[BOOTLOADER: GRUB & Auto-Unlock](../phases/BOOTLOADER.md)**
    *   Install and configure GRUB bootloader with LUKS auto-unlock.
6.  **[NETWORK: Wired & Wireless Setup](../phases/NETWORK.md)**
    *   Enable NetworkManager for network connectivity.
    *   Configure WiFi and Bluetooth support.
7.  **[AUDIO: PipeWire Sound Server](../phases/AUDIO.md)**
    *   Install and enable PipeWire for audio.
8.  **[SECURITY: Firewall & SSH Protection](../phases/SECURITY.md)**
    *   Configure UFW firewall.
    *   Install Fail2ban for SSH brute-force protection.
9.  **[Hardware (Optional)](../phases/HARDWARE.md)**
    *   Configure touchpad, webcam, IR camera, fingerprint reader if applicable.
10. **[FINALIZE: Exit & Reboot](../phases/FINALIZE.md)**
    *   Exit chroot, unmount, and reboot into your new system!

**Post-Installation Choices (after rebooting into your new system):**

*   **[AUR Helper (e.g., yay)](../wiki/POST-INSTALLATION.md#aur-helper-eg-yay)**: Highly recommended for easy access to community packages.
*   **[Display Server Configuration](../wiki/POST-INSTALLATION.md#display-servers)**: Xorg or Wayland.
*   **[Essential Applications](../wiki/POST-INSTALLATION.md#essential-applications)**: Install web browser, office suite, etc.
*   **[Backup Solution (Timeshift)](../wiki/POST-INSTALLATION.md#backup-solutions)**: Set up system snapshots.
*   **[Desktop Environment](../wiki/POST-INSTALLATION.md#desktop-environments)**: Install GNOME, KDE Plasma, XFCE, or a Window Manager.

---

### Standard Installation without Encryption

This flow covers a basic Arch Linux installation suitable for most users who do not require disk encryption.

**Recommended for:** Users who want a straightforward Arch Linux setup on a single disk.

**Phase Overview:**

1.  **[PREPARATION: Create USB & Format Disk](../phases/PREPARATION.md)**
    *   Create bootable Arch Linux USB.
    *   (Optional) Format entire disk for a fresh start.
    *   Boot from USB and connect to the network.
2.  **[DISK SETUP: Partitioning & Mount](../phases/DISK_SETUP.md)**
    *   Partition your disk (EFI, Root, Swap).
    *   Create filesystems (e.g., Ext4).
    *   Mount all partitions correctly.
3.  **[SYSTEM INSTALL: Base System & Fstab](../phases/SYSTEM_INSTALL.md)**
    *   Install the base Arch Linux system using `pacstrap`.
    *   Generate `fstab`.
4.  **[BASIC CONFIGURATION: Locale, User, Sudo](../phases/BASIC_CONFIG.md)**
    *   Set system locale and timezone.
    *   Create your user account and configure `sudo` access.
5.  **[BOOTLOADER: GRUB](../phases/BOOTLOADER.md)**
    *   Install and configure GRUB bootloader.
6.  **[NETWORK: Wired & Wireless Setup](../phases/NETWORK.md)**
    *   Enable NetworkManager for network connectivity.
    *   Configure WiFi and Bluetooth support.
7.  **[AUDIO: PipeWire Sound Server](../phases/AUDIO.md)**
    *   Install and enable PipeWire for audio.
8.  **[SECURITY: Firewall & SSH Protection](../phases/SECURITY.md)**
    *   Configure UFW firewall.
    *   Install Fail2ban for SSH brute-force protection.
9.  **[FINALIZE: Exit & Reboot](../phases/FINALIZE.md)**
    *   Exit chroot, unmount, and reboot into your new system!

**Post-Installation Choices (after rebooting into your new system):**

*   **[AUR Helper (e.g., yay)](../wiki/POST-INSTALLATION.md#aur-helper-eg-yay)**: Highly recommended for easy access to community packages.
*   **[Display Server Configuration](../wiki/POST-INSTALLATION.md#display-servers)**: Xorg or Wayland.
*   **[Essential Applications](../wiki/POST-INSTALLATION.md#essential-applications)**: Install web browser, office suite, etc.
*   **[Backup Solution (Timeshift)](../wiki/POST-INSTALLATION.md#backup-solutions)**: Set up system snapshots.
*   **[Desktop Environment](../wiki/POST-INSTALLATION.md#desktop-environments)**: Install GNOME, KDE Plasma, XFCE, or a Window Manager.
*   **[Hardware (Optional)](../phases/HARDWARE.md)**: Configure touchpad, webcam, IR camera, fingerprint reader if applicable.

---

### Dual Boot Installation

This flow focuses on installing Arch Linux alongside an existing Windows installation. For comprehensive guidance on dual-booting, refer to the [ArchWiki on Dual boot with Windows](https://wiki.archlinux.org/title/Dual_boot_with_Windows).

**Recommended for:** Users who want to keep Windows and add Arch Linux to their system.

**Phase Overview:**

1.  **[PREPARATION: Create USB & Prepare Disk](../phases/PREPARATION.md)**
    *   Ensure Windows is installed and free space is available for Arch Linux.
    *   Create bootable Arch Linux USB.
    *   Boot from USB and connect to the network.
2.  **[DISK SETUP: Partitioning & Mount](../phases/DISK_SETUP.md)**
    *   Partition the free space for Arch Linux (Root, Swap). **Use the existing Windows EFI partition.**
    *   Create filesystems (e.g., Ext4).
    *   Mount all partitions correctly (including Windows EFI).
3.  **[SYSTEM INSTALL: Base System & Fstab](../phases/SYSTEM_INSTALL.md)**
    *   Install the base Arch Linux system using `pacstrap`.
    *   Generate `fstab`.
4.  **[BASIC CONFIGURATION: Locale, User, Sudo](../phases/BASIC_CONFIG.md)**
    *   Set system locale and timezone.
    *   Create your user account and configure `sudo` access.
5.  **[BOOTLOADER: GRUB & OS Prober](../phases/BOOTLOADER.md)**
    *   Install and configure GRUB bootloader.
    *   **Crucially, ensure `os-prober` is configured and enabled to detect Windows.**
6.  **[NETWORK: Wired & Wireless Setup](../phases/NETWORK.md)**
    *   Enable NetworkManager for network connectivity.
    *   Configure WiFi and Bluetooth support.
7.  **[AUDIO: PipeWire Sound Server](../phases/AUDIO.md)**
    *   Install and enable PipeWire for audio.
8.  **[SECURITY: Firewall & SSH Protection](../phases/SECURITY.md)**
    *   Configure UFW firewall.
    *   Install Fail2ban for SSH brute-force protection.
9.  **[FINALIZE: Exit & Reboot](../phases/FINALIZE.md)**
    *   Exit chroot, unmount, and reboot into your new system!

**Post-Installation Choices (after rebooting into your new system):**

*   **[AUR Helper (e.g., yay)](../wiki/POST-INSTALLATION.md#aur-helper-eg-yay)**: Highly recommended for easy access to community packages.
*   **[Display Server Configuration](../wiki/POST-INSTALLATION.md#display-servers)**: Xorg or Wayland.
*   **[Essential Applications](../wiki/POST-INSTALLATION.md#essential-applications)**: Install web browser, office suite, etc.
*   **[Backup Solution (Timeshift)](../wiki/POST-INSTALLATION.md#backup-solutions)**: Set up system snapshots.
*   **[Desktop Environment](../wiki/POST-INSTALLATION.md#desktop-environments)**: Install GNOME, KDE Plasma, XFCE, or a Window Manager.
*   **[Hardware (Optional)](../phases/HARDWARE.md)**: Configure touchpad, webcam, IR camera, fingerprint reader if applicable.

---

### Minimal Server Installation

This flow provides a bare-bones Arch Linux installation, ideal for servers or users who want to build their system from scratch with no graphical environment.

**Recommended for:** Experienced users, servers, or those who prefer a highly customized, minimal setup.

**Phase Overview:**

1.  **[PREPARATION: Create USB & Format Disk](../phases/PREPARATION.md)**
    *   Create bootable Arch Linux USB.
    *   (Optional) Format entire disk for a fresh start.
    *   Boot from USB and connect to the network.
2.  **[DISK SETUP: Partitioning & Mount](../phases/DISK_SETUP.md)**
    *   Partition your disk (EFI, Root, Swap).
    *   Create filesystems (e.g., Ext4).
    *   Mount all partitions correctly.
3.  **[SYSTEM INSTALL: Base System & Fstab](../phases/SYSTEM_INSTALL.md)**
    *   Install the base Arch Linux system using `pacstrap`.
    *   Generate `fstab`.
4.  **[BASIC CONFIGURATION: Locale, User, Sudo](../phases/BASIC_CONFIG.md)**
    *   Set system locale and timezone.
    *   Create your user account and configure `sudo` access.
5.  **[BOOTLOADER: GRUB](../phases/BOOTLOADER.md)**
    *   Install and configure GRUB bootloader.
6.  **[NETWORK: Wired Setup](../phases/NETWORK.md)**
    *   Enable NetworkManager for network connectivity (focus on wired).
7.  **[SECURITY: Firewall & SSH Server](../phases/SECURITY.md)**
    *   Configure UFW firewall.
    *   Install and configure SSH server.
    *   Install Fail2ban for SSH brute-force protection.
8.  **[FINALIZE: Exit & Reboot](../phases/FINALIZE.md)**
    *   Exit chroot, unmount, and reboot into your new system!

**Post-Installation Choices (after rebooting into your new system):**

*   **[AUR Helper (e.g., yay)](../wiki/POST-INSTALLATION.md#aur-helper-eg-yay)**: Recommended for easy access to community packages.
*   Install any specific server applications or services as needed.

---

## 🛠️ Build Your Own Custom Flow

If none of the above scenarios perfectly match your requirements, you can build your own customized installation procedure:

1.  **Review Available Modules:** Browse the **[Module Index](../MODULE_INDEX.md)** to understand all individual components and their purposes.
2.  **Understand Phases:** Look at the **[Phases](../phases/PREPARATION.md)** directory for a logical grouping of steps.
3.  **Consult Steps:** The **[Step Index](../STEP_INDEX.md)** provides a quick reference for all individual commands.
4.  **Generate a Procedure:** If you have very specific needs, consider how to **[Generate a Custom Procedure](../GENERATE_PROCEDURE.md)** using the modular files.

---

**Back to:** [Home](../HOME.md) | **Complete Guide:** [../COMPLETE-INSTALLATION.md](../COMPLETE-INSTALLATION.md)
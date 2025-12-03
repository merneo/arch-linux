# Module: WiFi Configuration

**Purpose:** Install and configure WiFi support. WiFi (Wireless Fidelity) enables devices to connect to a network wirelessly using IEEE 802.11 standards. This module guides you through identifying your wireless adapter, installing necessary drivers and firmware, and configuring network access. For a comprehensive guide on wireless networking in Arch Linux, refer to the [ArchWiki on Wireless network configuration](https://wiki.archlinux.org/title/Wireless_network_configuration).

**Prerequisites:**
- Inside chroot environment (module `chroot.md`)
- NetworkManager enabled (module `networkmanager.md`)

**Time:** 2-3 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## How to know if you have a WiFi adapter?

Most laptops and many desktop motherboards include integrated WiFi. You can confirm its presence and identify its chipset by:
-   **Physical inspection:** Look for internal antennas or an external antenna connector.
-   **Laptop/Motherboard specifications:** Check your device's model specifications online.
-   **System detection commands:** Use `lspci` (for PCI-E/internal cards) and `lsusb` (for USB adapters) to identify the vendor and model of your wireless card. This is crucial for determining which drivers/firmware are needed.

```bash
# For PCI-E based WiFi cards (most internal laptop cards):
lspci -k | grep -EA3 'Network|Wireless'

# For USB WiFi adapters:
lsusb | grep -i wireless
```

---

## Step 1: Install WiFi Packages

Identifying your WiFi chipset (using `lspci` or `lsusb` as shown above) is crucial to ensure the correct drivers and firmware are installed. Most modern chipsets have in-kernel drivers, but often require additional firmware.

```bash
pacman -S wireless_tools wpa_supplicant dialog
```

**Package breakdown:**
- `wireless_tools`: A collection of utilities for configuring wireless network interfaces. See [ArchWiki: Wireless tools](https://wiki.archlinux.org/title/Wireless_tools).
- `wpa_supplicant`: An implementation of the WPA Supplicant, which is a key component for WPA/WPA2 authentication. See [ArchWiki: wpa_supplicant](https://wiki.archlinux.org/title/WPA_supplicant).
- `dialog`: A utility that helps to display dialog boxes from shell scripts, often used for text-based UI configuration tools like `nmtui`. See [ArchWiki: Dialog](https://wiki.archlinux.org/title/Dialog).

**Firmware and Driver Considerations by Chipset:**

*   **Intel Wireless (Intel Wi-Fi 6, AX200, AX201, etc.):** Typically supported by the `iwlwifi` kernel module and firmware provided by the `linux-firmware` package. These are usually installed by default, but ensuring `linux-firmware` is up-to-date is important.
*   **Broadcom Wireless (BCM43xx series):** Some Broadcom chipsets require proprietary drivers. The `broadcom-wl` package (available in AUR or via `dkms` for specific kernels) is often needed. Alternatively, the `b43-fwcutter` and `b43-firmware` packages might be used for older chips.
*   **Realtek Wireless (RTL8812au, RTL8192eu, etc.):** Many Realtek adapters require out-of-tree drivers, often available as DKMS packages in the AUR (e.g., `rtl88xxau-dkms-git`). Firmware for some models is in `linux-firmware`.
*   **Qualcomm Atheros Wireless (AR9xxx, QCA6174, etc.):** Generally well-supported by in-kernel `ath9k` or `ath10k` modules, with firmware from `linux-firmware`.

**Crucial Firmware Package:**
Ensure the `linux-firmware` package is installed, as it contains firmware for a vast range of wireless chipsets. It should have been installed during the base system installation.

```bash
# Ensure linux-firmware is installed
sudo pacman -S linux-firmware
```

---

## Step 2: Verify WiFi Support

After reboot, you will use NetworkManager to connect to WiFi networks. Command-line tools like `nmcli` and text-based user interfaces like `nmtui` provide flexible ways to manage your wireless connections. For usage details, refer to the [ArchWiki on nmcli](https://wiki.archlinux.org/title/NetworkManager#nmcli) and [ArchWiki on nmtui](https://wiki.archlinux.org/title/NetworkManager#nmtui).

**Command line:**
```bash
nmcli device wifi list
nmcli device wifi connect "SSID" password "password"
```

**Text UI:**
```bash
nmtui
```

**GUI:**
- NetworkManager applet (if desktop environment installed)

---

## Troubleshooting

For more extensive troubleshooting on wireless issues, refer to the [ArchWiki on Wireless network configuration#Troubleshooting](https://wiki.archlinux.org/title/Wireless_network_configuration#Troubleshooting) and the [ArchWiki on Network configuration#Troubleshooting](https://wiki.archlinux.org/title/Network_configuration#Troubleshooting).

### Problem: WiFi adapter not detected or not working
**Solution:**
1. **Verify hardware detection:**
   - For PCI-E cards: `lspci -k | grep -EA3 'Network|Wireless'`
   - For USB adapters: `lsusb | grep -i wireless`
   - Check if kernel module is loaded: `lsmod | grep -E 'iwlwifi|ath|rtw'` (replace with your likely driver)
2. **Check dmesg for errors:** `dmesg | grep -i 'firmware\|wifi\|wlan'`
3. **Ensure correct firmware is installed:** If your card needs specific firmware not in `linux-firmware` (e.g., some Broadcom chips), install it.
4. **Check if adapter is blocked:** `rfkill list all` (unblock if necessary).
5. **Reboot:** Sometimes a fresh boot helps load drivers/firmware correctly.

### Problem: Slow WiFi speeds or drops
**Solution:**
1. **Check signal strength:** Use `nmcli device wifi list` or `iwconfig` (from `wireless_tools`).
2. **Interference:** Move closer to the router or check for other wireless devices.
3. **Driver issues:** Ensure you have the latest drivers/firmware. Sometimes, using a different kernel (e.g., `linux-lts`) can help.
4. **NetworkManager configuration:** Review NetworkManager connection settings for issues.

---

**SUCCESS:** WiFi support installed

**Note:** WiFi configuration happens after first boot using NetworkManager

**Next:** Continue with other configuration modules
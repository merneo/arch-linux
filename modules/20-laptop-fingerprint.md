# Module: Laptop Fingerprint Reader Configuration

**Purpose:** Configure fingerprint reader for laptops (Validity Sensors, Synaptics, etc.)

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`) OR after first boot
- Laptop hardware (not desktop)
- Fingerprint reader hardware present
- **CRITICAL:** BIOS reset may be required (see Step 1)

**Time:** 20-30 minutes

**ENVIRONMENT:** Chroot (root@archiso /)# OR After first boot

**Note:** This module is for **LAPTOPS only**. Desktop computers typically don't have built-in fingerprint readers.

---

## ⚠️ CRITICAL: BIOS Reset (May Be Required)

**Some fingerprint readers (especially Validity Sensors 138a:0092) require BIOS reset before they work.**

1. Restart → Press **F10** (or appropriate key) during boot
2. Navigate: **Security → Fingerprint Reader Reset**
3. Enable: "Reset fingerprint reader on next boot"
4. Save and restart
5. Wait 1-2 minutes during boot (reset in progress)

**Skip this step if your fingerprint reader works without reset.**

---

## Step 1: Verify Fingerprint Reader Detection

Verify that your system detects the fingerprint reader as a USB device. The `lsusb` command lists USB devices and their IDs. For details, refer to the [ArchWiki on lsusb](https://wiki.archlinux.org/title/USB#lsusb).

```bash
# List USB devices
lsusb | grep -i "fingerprint\|validity"

# Expected output (example):
# Bus 001 Device 003: ID 138a:0092 Validity Sensors, Inc.

# Check device ID (note the vendor:product ID)
```

---

## Step 2: Identify Your Fingerprint Reader Model

Identifying the specific model of your fingerprint reader is crucial as some require device-specific drivers or configurations. The `lsusb -v` command provides verbose details about USB devices.

**Common models:**
- **Validity Sensors 138a:0092** (Synaptics VFS7552) - requires device/0092 branch
- **Validity Sensors 138a:0017** (Synaptics VFS5011)
- **Upek TouchChip** (various models)
- **AuthenTec** (various models)

**Check your model (for more verbose output, refer to [ArchWiki: lsusb](https://wiki.archlinux.org/title/USB#lsusb)):**
```bash
lsusb -v | grep -A 5 "idVendor.*138a"
```

---

## Step 3: Install Required Packages

Depending on your fingerprint reader model, you will typically need a specific driver (like `python-validity`) and the `fprintd` daemon. `yay` is an [AUR helper](https://wiki.archlinux.org/title/AUR_helpers) used to install packages from the Arch User Repository. `fprintd` is a system daemon that provides a D-Bus interface for fingerprint readers. For more information, refer to the [ArchWiki on Fprint](https://wiki.archlinux.org/title/Fprint).

### For Validity Sensors (most common):

```bash
# Install from AUR
yay -S python-validity-git

# Install fprintd (fingerprint daemon)
pacman -S fprintd
```

### For other fingerprint readers:

```bash
# Install fprintd (supports many models)
pacman -S fprintd

# Check if your model is supported:
fprintd-list $USER
```

---

## Step 4: Install Device-Specific Support (if needed)

Some fingerprint readers, particularly certain Validity Sensors models, require specific branches or patches to their drivers. This step involves cloning a specific Git repository branch and manually copying files. For basic usage of Git, refer to the [ArchWiki on Git](https://wiki.archlinux.org/title/Git).

**For Validity Sensors 138a:0092 (requires special branch):**

```bash
# Clone device/0092 branch
cd /tmp
git clone -b device/0092 https://github.com/uunicorn/python-validity.git python-validity-0092

# Find python-validity installation location
VALIDITY_DIR=$(python3 -c "import site; print(site.getsitepackages()[0])")/validitysensor

# Backup original files
sudo cp -r $VALIDITY_DIR ${VALIDITY_DIR}.backup

# Copy device/0092 branch files
sudo cp -r /tmp/python-validity-0092/validitysensor/* $VALIDITY_DIR/

# Verify blobs_92.py exists
ls -la $VALIDITY_DIR/blobs_92.py
```

---

## Step 5: Configure udev Rules

[udev](https://wiki.archlinux.org/title/Udev) provides a dynamic device management system for Linux. Custom udev rules are often necessary to grant appropriate permissions to non-root users for specific hardware, such as fingerprint readers. Adding the user to the `input` group grants access to input devices. See [ArchWiki: Users and groups](https://wiki.archlinux.org/title/Users_and_groups) for group management.

```bash
# Create udev rules for fingerprint reader
sudo tee /etc/udev/rules.d/60-validity.rules << 'EOF'
# Validity Sensors - broad match
SUBSYSTEMS=="usb", ATTRS{idVendor}=="138a", MODE="0666"

# Validity Sensors 138a:0092 - specific match
SUBSYSTEMS=="usb", ATTRS{idVendor}=="138a", ATTRS{idProduct}=="0092", MODE="0666"
EOF

# Reload udev rules
sudo udevadm control --reload
sudo udevadm trigger

# Add user to input group
sudo usermod -a -G input $USER
```

---

## Step 6: Start Fingerprint Service

Services in Arch Linux are managed by `systemd`. The `systemctl enable --now` command enables a service to start at boot and immediately starts it. For more details on `systemd` and service management, refer to the [ArchWiki on systemd](https://wiki.archlinux.org/title/Systemd).

```bash
# Enable and start python3-validity service
sudo systemctl enable --now python3-validity

# Verify service is running
systemctl status python3-validity
```

---

## Step 7: Verify Detection

After starting the `fprintd` service, you should verify that it detects your fingerprint device. The `fprintd-list` command provides information about detected fingerprint readers. For more details, consult `man fprintd` or the [ArchWiki on Fprint](https://wiki.archlinux.org/title/Fprint).

```bash
# List available fingerprint devices
fprintd-list $USER

# Expected output:
# found 1 devices
# Device /org/freedesktop/fprintd/Device/0
```

---

## Step 8: Enroll Fingerprint

Enrolling your fingerprint registers it with the system, allowing it to be used for authentication. The `fprintd-enroll` command guides you through this process. For more details, consult `man fprintd-enroll` or the [ArchWiki on Fprint](https://wiki.archlinux.org/title/Fprint).

```bash
# Enroll fingerprint
fprintd-enroll $USER

# Follow prompts:
# 1. Place finger on sensor
# 2. Lift and place again (repeat 5-10 times)
# 3. Wait for "Enroll result: enroll-completed"
```

---

## Step 9: Test Verification

After enrollment, it's important to test that the fingerprint reader can successfully verify your enrolled fingerprint. The `fprintd-verify` command facilitates this testing. For more details, consult `man fprintd-verify` or the [ArchWiki on Fprint](https://wiki.archlinux.org/title/Fprint).

```bash
# Test fingerprint verification
fprintd-verify $USER

# Place enrolled finger on sensor
# Expected: "Verify result: verify-match (done)"
```

---

## Step 10: Configure PAM (Optional - for Authentication)

[PAM (Pluggable Authentication Modules)](https://wiki.archlinux.org/title/PAM) allows for flexible configuration of authentication methods. Integrating `pam_fprintd.so` enables fingerprint authentication for various system services.

**For sudo authentication:** This allows using your fingerprint instead of a password for `sudo` commands. See [ArchWiki: Sudo#Fingerprint_authentication](https://wiki.archlinux.org/title/Sudo#Fingerprint_authentication).

```bash
sudo nano /etc/pam.d/sudo
```

Add at the top:
```
auth      sufficient  pam_fprintd.so
```

**For SDDM login:** If you are using SDDM as your display manager, you can integrate fingerprint for login authentication. See [ArchWiki: SDDM#Fingerprint_authentication](https://wiki.archlinux.org/title/SDDM#Fingerprint_authentication).

```bash
sudo nano /etc/pam.d/sddm
```

Add at the top:
```
auth      sufficient  pam_fprintd.so
```

---

## Troubleshooting

For more extensive troubleshooting on fingerprint reader issues, refer to the [ArchWiki on Fprint#Troubleshooting](https://wiki.archlinux.org/title/Fprint#Troubleshooting).

### Problem: Fingerprint reader not detected
**Solution:**
1. Check USB: `lsusb | grep -i "fingerprint\|validity"`
2. Try BIOS reset (see Step 1)
3. Check udev rules: `ls -la /etc/udev/rules.d/60-validity.rules`
4. Reload udev: `sudo udevadm control --reload && sudo udevadm trigger`

### Problem: fprintd-list shows "found 0 devices"
**Solution:**
1. Check service: `systemctl status python3-validity`
2. Check logs: `journalctl -u python3-validity`. For `journalctl` details, see [ArchWiki: Systemd/Journal](https://wiki.archlinux.org/title/Systemd/Journal).
3. Verify device-specific support is installed (for 138a:0092)
4. Try BIOS reset

### Problem: Enrollment fails
**Solution:**
1. Clean sensor surface
2. Ensure finger is dry and clean
3. Try different finger
4. Check service logs: `journalctl -u python3-validity`

---

**SUCCESS:** Fingerprint reader configured and working

**Official Resources:**
- [ArchWiki: Fprint](https://wiki.archlinux.org/title/Fprint)
- [python-validity GitHub](https://github.com/uunicorn/python-validity)
- [fprintd Documentation](https://fprint.freedesktop.org/)

**Next:** Continue with other configuration modules

# Generate Installation Procedure

**Purpose:** Guide to generate a custom installation procedure based on your needs

---

## Step 1: Assess Your Situation

Answer these questions:

### Disk Status
- [ ] Disk is already partitioned → Skip `06-disk-partitioning.md`
- [ ] Disk needs partitioning → Include `06-disk-partitioning.md`

### Encryption
- [ ] Want disk encryption → Include `07-luks-encryption.md`
- [ ] Want automatic unlock → Include `10-luks-keyfile-auto-unlock.md`
- [ ] No encryption needed → Skip LUKS modules

### Filesystem
- [ ] Want Btrfs with subvolumes → Include `08-btrfs-filesystem.md`
- [ ] Want ext4/xfs → Include `09-mount-partitions.md`
- [ ] Filesystem already created → Skip filesystem modules

### Basic Configuration
- [ ] Need locale/timezone → Include `02-locale.md`
- [ ] Need root password → Include `03-root-password.md` (recommended)
- [ ] Need user account → Include `04-user-creation.md` (recommended)

### Bootloader
- [ ] Need GRUB → Include `05-grub.md` (required)

### Network
- [ ] Need NetworkManager → Include `11-networkmanager.md` (recommended)
- [ ] Need WiFi → Include `12-wifi.md`
- [ ] Need Bluetooth → Include `13-bluetooth.md`

### Audio
- [ ] Need audio → Include `14-audio.md`

---

## Step 2: Build Your Procedure

Start with these **ALWAYS REQUIRED** modules:

1. `00-core-installation.md` - Install base system
2. `01-chroot.md` - Enter chroot

Then add modules based on your answers above, in this order:

### If disk needs partitioning:
- `06-disk-partitioning.md` - Create partitions

### If using encryption:
- `07-luks-encryption.md` - Encrypt partitions

### If using Btrfs:
- `08-btrfs-filesystem.md` - Create Btrfs (works with or without LUKS)

### If using ext4/xfs:
- `09-mount-partitions.md` - Mount partitions

### Configuration (in chroot):
- `02-locale.md` - Set locale
- `03-root-password.md` - Set root password
- `04-user-creation.md` - Create user
- `05-grub.md` - Install GRUB

### Advanced (optional):
- `10-luks-keyfile-auto-unlock.md` - Auto-unlock (if using LUKS)
- `11-networkmanager.md` - Enable NetworkManager
- `12-wifi.md` - WiFi support
- `13-bluetooth.md` - Bluetooth support
- `14-audio.md` - Audio server

### Final step:
- `15-exit-chroot.md` - Exit and reboot

---

## Step 3: Example Procedures

### Scenario 1: New PC, Disk Already Partitioned, No Encryption

**Modules needed:**
1. `09-mount-partitions.md` - Mount existing partitions
2. `00-core-installation.md` - Install base system
3. `01-chroot.md` - Enter chroot
4. `02-locale.md` - Set locale
5. `03-root-password.md` - Set root password
6. `04-user-creation.md` - Create user
7. `05-grub.md` - Install GRUB
8. `11-networkmanager.md` - Enable NetworkManager
9. `15-exit-chroot.md` - Exit and reboot

### Scenario 2: New PC, Need Full Setup (LUKS + Btrfs)

**Modules needed:**
1. `06-disk-partitioning.md` - Create partitions
2. `07-luks-encryption.md` - Encrypt partitions
3. `08-btrfs-filesystem.md` - Create Btrfs
4. `00-core-installation.md` - Install base system
5. `01-chroot.md` - Enter chroot
6. `02-locale.md` - Set locale
7. `03-root-password.md` - Set root password
8. `04-user-creation.md` - Create user
9. `05-grub.md` - Install GRUB (with LUKS parameters)
10. `10-luks-keyfile-auto-unlock.md` - Auto-unlock (optional)
11. `11-networkmanager.md` - Enable NetworkManager
12. `12-wifi.md` - WiFi support
13. `14-audio.md` - Audio server
14. `15-exit-chroot.md` - Exit and reboot

### Scenario 3: Minimal Installation (Just System + Root)

**Modules needed:**
1. `00-core-installation.md` - Install base system (assumes `/mnt` is already mounted)
2. `01-chroot.md` - Enter chroot
3. `03-root-password.md` - Set root password
4. `05-grub.md` - Install GRUB
5. `15-exit-chroot.md` - Exit and reboot

---

## Step 4: Verify Dependencies

Before starting, check each module's prerequisites:

- Most modules require `01-chroot.md` (you must be in chroot)
- GRUB requires partitions to be mounted
- LUKS modules require disk partitioning
- Btrfs can work with or without LUKS

---

## Step 5: Execute Your Procedure

1. **Read each module** before executing
2. **Follow steps in order** (respecting dependencies)
3. **Verify success** after each module
4. **Customize** commands (usernames, timezones, UUIDs, etc.)

---

## Tips

- **Start simple:** Begin with minimal installation, add features later
- **Read prerequisites:** Each module lists what's needed before it
- **Check MODULE_INDEX.md:** Quick reference for all modules
- **Customize:** Replace placeholders (usernames, UUIDs, etc.) with your values
- **Verify:** Check success messages after each module

---

**Remember:** This system assumes you're already booted from Arch Linux Live USB. It doesn't format disks or create USB drives - it only installs and configures the system.

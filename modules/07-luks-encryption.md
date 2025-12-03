# Module: LUKS2 Encryption Setup

**Purpose:** Encrypt disk partitions with LUKS2

**Prerequisites:**
- Disk partitioned (module `06-disk-partitioning.md`)
- Root partition identified (e.g., `<ROOT_PARTITION>`)

**Time:** 10-15 minutes

**ENVIRONMENT:** Live USB (root@archiso)

---

## ⚠️ WARNING: Data Loss

**This module will PERMANENTLY DELETE all data on the target partition(s).**

- Make sure you have backups of important data
- Double-check partition names before proceeding (use `lsblk` to verify)
- This operation CANNOT be undone

---

## Security Notes

- **LUKS passphrase:** Use a strong passphrase (minimum 20 characters, mix of letters, numbers, symbols)
- **Never share your passphrase** - it cannot be recovered if lost
- **Backup keyfile** if using auto-unlock (store securely, not on the same disk)
- **Remember your passphrase** - without it, encrypted data is permanently inaccessible

---

## Placeholders Used in This Module

- `<ROOT_PARTITION>`: Your root partition device (e.g., `/dev/sda2`, `/dev/nvme0n1p2`)
- `<SWAP_PARTITION>`: Your swap partition device (e.g., `/dev/sda3`, `/dev/nvme0n1p3`)

---

## Step 1: Encrypt Root Partition

```bash
# Replace /dev/sdX2 with your root partition
cryptsetup luksFormat --type luks2 \
  --cipher aes-xts-plain64 \
  --key-size 512 \
  --pbkdf argon2id \
  --iter-time 5000 \
  /dev/sdX2
```

**Prompt 1: Confirmation**
```
WARNING!
========
This will overwrite data on /dev/sdX2 irrevocably.

Are you sure? (Type 'yes' in capital letters):
```

**Type exactly:** `YES` (uppercase) and press Enter

**Prompt 2: Passphrase**
```
Enter passphrase for /dev/sdX2:
```

**Enter a STRONG passphrase** (minimum 20 characters recommended)

**Prompt 3: Verify passphrase**
```
Verify passphrase:
```

**Re-type the EXACT SAME passphrase** and press Enter

**Wait for completion** (30-60 seconds)

---

## Step 2: Open Encrypted Root

```bash
# Unlock LUKS container and map to /dev/mapper/cryptroot
cryptsetup open /dev/sdX2 cryptroot

# Enter the passphrase you just created
```

**Verify:**
```bash
ls -la /dev/mapper/

# Should show:
# cryptroot -> ../dm-0
```

---

## Step 3: Encrypt Swap Partition (Optional)

```bash
# Replace /dev/sdX3 with your swap partition
cryptsetup luksFormat --type luks2 \
  --cipher aes-xts-plain64 \
  --key-size 512 \
  --pbkdf argon2id \
  --iter-time 5000 \
  /dev/sdX3

# Type YES, enter passphrase (can be same as root)

# Open encrypted swap
cryptsetup open /dev/sdX3 cryptswap
```

---

## Step 4: Verify Encryption

```bash
# Display LUKS header information
cryptsetup luksDump /dev/sdX2 | head -20

# Should show:
# LUKS header information for /dev/sdX2
# Version:        2
# Cipher name:    aes
# Cipher mode:    xts-plain64
# Hash spec:      sha512
# Key Slot 0: ENABLED
```

---

**SUCCESS:** Partitions encrypted with LUKS2

## Verification

Run these commands to verify encryption:

```bash
# Display LUKS header information
cryptsetup luksDump <ROOT_PARTITION> | head -20

# Should show:
# LUKS header information for <ROOT_PARTITION>
# Version:        2
# Cipher name:    aes
# Cipher mode:    xts-plain64
# Key Slot 0: ENABLED

# Verify LUKS container can be opened
cryptsetup open <ROOT_PARTITION> cryptroot
# Enter passphrase - should succeed without errors

# Check mapper device exists
ls -la /dev/mapper/cryptroot

# Should show: cryptroot -> ../dm-0
```

**Next:**
- `08-btrfs-filesystem.md` - Create Btrfs filesystem on encrypted partition
- `09-mount-partitions.md` - Mount partitions before installation

---

## Troubleshooting

### Problem: cryptsetup fails with "Device already in use"
**Solution:**
1. Check if LUKS container is already open: `ls /dev/mapper/`
2. Close if needed: `cryptsetup close cryptroot`
3. Retry encryption

### Problem: "No key available" when opening LUKS
**Solution:**
1. Verify you're using the correct passphrase
2. Check if key slot is enabled: `cryptsetup luksDump <PARTITION> | grep "Key Slot"`
3. If key slot is disabled, you may need to re-encrypt (data will be lost)

### Problem: Forgot LUKS passphrase
**Solution:**
- **WARNING:** If you forgot the passphrase, encrypted data is permanently inaccessible
- There is no recovery method for forgotten LUKS passphrases
- You will need to re-encrypt the partition (all data will be lost)

# Module: LUKS2 Encryption Setup

**Purpose:** Encrypt disk partitions with LUKS2. LUKS (Linux Unified Key Setup) is the standard for Linux hard disk encryption, offering a robust and secure way to protect data at rest. LUKS2 is the newer version with improved features and flexibility. For a comprehensive guide, refer to the [ArchWiki on dm-crypt/Encrypting an entire system](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system).

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

The `cryptsetup luksFormat` command initializes a LUKS encrypted volume. The parameters specify the encryption algorithm and key derivation function.

-   `--type luks2`: Specifies the LUKS2 format, offering advanced features over LUKS1.
-   `--cipher aes-xts-plain64`: Uses [AES (Advanced Encryption Standard)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) in XTS mode with a 512-bit key. AES is a widely adopted symmetric encryption algorithm.
-   `--key-size 512`: Sets the key size to 512 bits.
-   `--pbkdf argon2id`: Uses [Argon2id](https://en.wikipedia.org/wiki/Argon2) as the Password-Based Key Derivation Function, which is designed to resist GPU cracking attacks.
-   `--iter-time 5000`: Sets the iteration time for the PBKDF to 5000 milliseconds, increasing the effort required for brute-force attacks.

For more details on `cryptsetup` and its options, refer to the [ArchWiki on dm-crypt/Drive encryption](https://wiki.archlinux.org/title/Dm-crypt/Drive_encryption#Encryption_options) or consult `man cryptsetup`.

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

After encrypting a partition with `luksFormat`, it must be "opened" to make the underlying unencrypted device available. The `cryptsetup open` command decrypts the volume and maps it to a device in `/dev/mapper/`. For more details, refer to the [ArchWiki on dm-crypt/Device encryption](https://wiki.archlinux.org/title/Dm-crypt/Device_encryption#Using_dm-crypt).

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

Encrypting the swap partition is a recommended security measure to prevent sensitive data from being written to unencrypted storage. For best practices regarding swap encryption, refer to the [ArchWiki on dm-crypt/Swap encryption](https://wiki.archlinux.org/title/Dm-crypt/Swap_encryption).

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

The `cryptsetup luksDump` command displays detailed information about a LUKS-encrypted volume, including its version, cipher, hash, and key slots. This is useful for verifying the encryption parameters. For more details, consult `man cryptsetup` or the [ArchWiki on dm-crypt/Drive encryption](https://wiki.archlinux.org/title/Dm-crypt/Drive_encryption).

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

For more extensive troubleshooting on `dm-crypt` related issues, refer to the [ArchWiki on dm-crypt/Troubleshooting](https://wiki.archlinux.org/title/Dm-crypt/Troubleshooting).

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

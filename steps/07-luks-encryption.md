# Step: LUKS2 Encryption Setup

**Purpose:** Encrypt disk partitions with LUKS2. LUKS is the standard for Linux hard disk encryption. For a comprehensive guide, refer to the [ArchWiki on dm-crypt/Encrypting an entire system](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system).

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Disk partitioned, root partition identified

## Commands

```bash
# Encrypt root partition (replace /dev/sdX2 with your root partition).
# See [ArchWiki: dm-crypt/Drive encryption#Encryption_options](https://wiki.archlinux.org/title/Dm-crypt/Drive_encryption#Encryption_options) for parameter details.
cryptsetup luksFormat --type luks2 \
  --cipher aes-xts-plain64 \
  --key-size 512 \
  --pbkdf argon2id \
  --iter-time 5000 \
  /dev/sdX2
# Type: YES (uppercase)
# Enter strong passphrase (min 20 chars)

# Open encrypted root partition. See [ArchWiki: dm-crypt/Device encryption](https://wiki.archlinux.org/title/Dm-crypt/Device_encryption#Using_dm-crypt).
cryptsetup open /dev/sdX2 cryptroot

# Encrypt swap partition (if exists, replace /dev/sdX3). See [ArchWiki: dm-crypt/Swap encryption](https://wiki.archlinux.org/title/Dm-crypt/Swap_encryption).
cryptsetup luksFormat --type luks2 /dev/sdX3
# Type: YES
# Enter passphrase (can be same as root)

# Open encrypted swap
cryptsetup open /dev/sdX3 cryptswap
```

## Verification

```bash
lsblk # Should show: cryptroot, cryptswap
cryptsetup luksDump /dev/sdX2 # Verify encryption details. See [ArchWiki: dm-crypt/Drive encryption](https://wiki.archlinux.org/title/Dm-crypt/Drive_encryption).
# Should show: Version: 2, Cipher: aes-xts-plain64
```

**NEXT:** `08-btrfs-filesystem.md` or `09-mount-partitions.md`

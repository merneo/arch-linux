# Step: LUKS2 Encryption Setup

**ENVIRONMENT:** Live USB (root@archiso)
**PREREQUISITES:** Disk partitioned, root partition identified

## Commands

```bash
# Encrypt root partition (replace /dev/sdX2 with your root partition)
cryptsetup luksFormat --type luks2 \
  --cipher aes-xts-plain64 \
  --key-size 512 \
  --pbkdf argon2id \
  --iter-time 5000 \
  /dev/sdX2
# Type: YES (uppercase)
# Enter strong passphrase (min 20 chars)

# Open encrypted root partition
cryptsetup open /dev/sdX2 cryptroot

# Encrypt swap partition (if exists, replace /dev/sdX3)
cryptsetup luksFormat --type luks2 /dev/sdX3
# Type: YES
# Enter passphrase (can be same as root)

# Open encrypted swap
cryptsetup open /dev/sdX3 cryptswap
```

## Verification

```bash
lsblk
# Should show: cryptroot, cryptswap

cryptsetup luksDump /dev/sdX2
# Should show: Version: 2, Cipher: aes-xts-plain64
```

**NEXT:** `08-btrfs-filesystem.md` or `09-mount-partitions.md`

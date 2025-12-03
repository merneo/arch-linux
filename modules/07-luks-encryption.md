# Module: LUKS2 Encryption Setup

**Purpose:** Encrypt disk partitions with LUKS2

**Prerequisites:**
- Disk partitioned (module `06-disk-partitioning.md`)
- Root partition identified (e.g., `/dev/sdX2`)

**Time:** 10-15 minutes

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

**Next:**
- `08-btrfs-filesystem.md` - Create Btrfs filesystem on encrypted partition
- `09-mount-partitions.md` - Mount partitions before installation

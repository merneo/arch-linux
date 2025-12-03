# Module: Create User Account

**Purpose:** Create non-root user account with sudo access

**Prerequisites:**
- Inside chroot environment (module `01-chroot.md`)
- Sudo package installed (included in `00-core-installation.md`)

**Time:** 2-3 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## Step 1: Create User

```bash
# Replace <USERNAME> with your desired username
useradd -m -G wheel,video,audio,storage,input,power,network -s /bin/bash <USERNAME>
```

**Placeholders:**
- `<USERNAME>`: Your desired username (lowercase, no spaces, e.g., `john`, `alice`)

# Explanation:
# -m: Create home directory (/home/username)
# -G: Add to supplementary groups:
#     wheel: sudo access
#     video: GPU access
#     audio: sound access
#     storage: removable media access
#     input: input devices
#     power: power management
#     network: network configuration
# -s /bin/bash: Default shell (or use /bin/zsh)
```

---

## Step 2: Set User Password

```bash
passwd <USERNAME>
```

**Enter password** for the user.

**Security Notes:**
- Use a strong password (minimum 12 characters recommended)
- Consider using a password manager
- This password is for daily use (less critical than root password, but still important)

---

## Step 3: Configure Sudo Access

```bash
EDITOR=nano visudo
```

**Find line:**
```bash
# %wheel ALL=(ALL:ALL) ALL
```

**Uncomment (remove #):**
```bash
%wheel ALL=(ALL:ALL) ALL
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

---

## Step 4: Verify User

```bash
id <USERNAME>

# Should show:
# uid=1000(<USERNAME>) gid=1000(<USERNAME>) groups=1000(<USERNAME>),wheel,video,audio,storage,input,power,network
```

**Verification:**
- User ID (uid) should be 1000 (first user)
- Group ID (gid) should match username
- Groups should include: wheel, video, audio, storage, input, power, network

---

**SUCCESS:** User created with sudo access

**Next:** Continue with other configuration modules

# Module: Create User Account

**Purpose:** Create non-root user account with sudo access. This is a fundamental security practice, as it prevents routine system use with root privileges. For comprehensive information on user management, refer to the [ArchWiki on Users and groups](https://wiki.archlinux.org/title/Users_and_groups) and [ArchWiki on Sudo](https://wiki.archlinux.org/title/Sudo).

**Prerequisites:**
- Inside chroot environment (module `chroot.md`)
- Sudo package installed (included in `core-installation.md`)

**Time:** 2-3 minutes

**ENVIRONMENT:** Chroot (root@archiso /)#

---

## Step 1: Create User

The `useradd` command creates a new user account. For more details on `useradd` and user management, refer to the [ArchWiki on useradd](https://wiki.archlinux.org/title/Users_and_groups#User_management).

```bash
# Replace <USERNAME> with your desired username
useradd -m -G wheel,video,audio,storage,power,network -s /bin/bash <USERNAME>
```

**Placeholders:**
- `<USERNAME>`: Your desired username (lowercase, no spaces, e.g., `john`, `alice`)

# Explanation:
# -m: Create home directory (/home/username)
# -G: Add to supplementary groups:
#     wheel: Provides sudo access. See [ArchWiki: Sudo](https://wiki.archlinux.org/title/Sudo).
#     video: Grants access to GPU hardware.
#     audio: Grants access to sound devices.
#     storage: Grants access to storage devices.
#     power: Allows user to manage power settings (e.g., shutdown, suspend).
#     network: Grants access to network configuration.
# -s /bin/bash: Default shell (or use /bin/zsh). See [ArchWiki: Command-line shell](https://wiki.archlinux.org/title/Command-line_shell).
```

---

## Step 2: Set User Password

The `passwd` command is used to change user passwords. For more details, refer to the [ArchWiki on passwd](https://wiki.archlinux.org/title/Passwd).

```bash
passwd <USERNAME>
```

**Enter password** for the user.

**Security Notes:**
- Use a strong password (minimum 12 characters recommended, mix of letters, numbers, symbols). For best practices, consult a guide on [Creating a strong password](https://www.us-cert.gov/ncas/tips/ST04-002).
- Consider using a password manager
- This password is for daily use (less critical than root password, but still important)

---

## Step 3: Configure Sudo Access

[Sudo](https://wiki.archlinux.org/title/Sudo) (Superuser Do) allows a permitted user to execute a command as the superuser or another user. It's a critical tool for system administration, enabling non-root users to perform privileged operations safely.

The `visudo` command provides a safe and recommended way to edit the `/etc/sudoers` file, which defines `sudo` policies. `visudo` performs syntax checking before saving, preventing common configuration errors that could lock out `sudo` access. For a detailed guide on configuring `sudo`, refer to the [ArchWiki on Sudo](https://wiki.archlinux.org/title/Sudo) and consult `man sudoers` for the file format.

```bash
EDITOR=nano visudo
```

**Find line:**
```bash
# %wheel ALL=(ALL:ALL) ALL
```

**Uncomment (remove #):** This line grants all members of the `wheel` group the ability to run any command as any user (including root) without needing to re-authenticate if they've recently provided their own password.
```bash
%wheel ALL=(ALL:ALL) ALL
```

**Save and exit** (Ctrl+O, Enter, Ctrl+X)

---

## Step 4: Verify User

The `id` command displays user and group information for the specified username. For more details on the `id` command, refer to its [man page](https://man.archlinux.org/man/id.1).

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

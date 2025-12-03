# Module: w3m Terminal Web Browser

**Purpose:** Install and configure `w3m`, a text-based web browser, essential for accessing web content in a terminal environment, especially useful in minimal installations or for troubleshooting.

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `04-user-creation.md`)
- Network connectivity (see `11-networkmanager.md`)
- (Optional) Terminal emulator (if not using default system console)

**Time:** 2-5 minutes

**ENVIRONMENT:** After first boot (logged in as user) or directly from TTY

---

## What is w3m?

`w3m` is a text-based web browser as well as a pager like `more` or `less`. It can display web pages in a terminal or console, making it invaluable for systems without a graphical desktop environment, for remote server administration, or for quick lookup of documentation during troubleshooting where a full browser is unavailable or undesirable. For more detailed information, refer to the [ArchWiki on w3m](https://wiki.archlinux.org/title/W3m).

---

## Step 1: Install w3m

`w3m` is available in the official Arch Linux repositories.

```bash
sudo pacman -S w3m
```

---

## Step 2: Basic Usage

After installation, you can launch `w3m` from your terminal or TTY.

```bash
w3m archlinux.org
```

**Key w3m Commands:**
*   **Arrow keys:** Scroll up/down, left/right.
*   **Tab / Shift+Tab:** Navigate between links.
*   **Enter:** Follow a link.
*   **B:** Go back.
*   **Shift+G:** Go to URL (prompts for a new URL).
*   **Q:** Quit `w3m`.

---

## Troubleshooting

### Problem: w3m cannot connect to websites
**Solution:**
1. **Verify network connectivity:** `ping archlinux.org`
2. **Check DNS resolution:** `dig archlinux.org`
3. **Check firewall rules:** Ensure outgoing connections are not blocked by UFW or other firewalls.

---

**SUCCESS:** `w3m` terminal web browser installed and ready for use.

**Official Resources:**
- [ArchWiki: w3m](https://wiki.archlinux.org/title/W3m)
- `man w3m` (consult the manual page for comprehensive options)

**Next:** Continue with other configuration modules or use `w3m` to browse documentation.

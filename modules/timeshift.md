# Module: Timeshift for System Snapshots

**Purpose:** Install and configure Timeshift to create and manage system snapshots, providing an easy way to restore your system to a previous working state. [Timeshift](https://github.com/teejee2008/timeshift) is a utility that creates incremental snapshots of the filesystem using `rsync` or Btrfs snapshots, allowing for convenient system rollback. For a comprehensive guide, refer to the [ArchWiki on Timeshift](https://wiki.archlinux.org/title/Timeshift).

**Prerequisites:**
- After first boot (not in chroot)
- User account created with sudo access (see `user-creation.md`)
- Network connectivity (see `networkmanager.md`)
- A separate partition or disk for snapshots is highly recommended, especially for Btrfs snapshots. For rsync snapshots, any sufficiently large drive can be used.

**Time:** 5-15 minutes (initial setup), plus snapshot creation time.

**ENVIRONMENT:** After first boot (logged in as user)

---

## Step 1: Install Timeshift

Timeshift is available in the official Arch Linux repositories.

```bash
sudo pacman -S timeshift
```

---

## Step 2: Configure Timeshift (GUI)

Timeshift is primarily a GUI tool for configuration, but it offers powerful underlying snapshot mechanisms.

1.  **Launch Timeshift:** Open your application launcher and search for "Timeshift".
2.  **Select Snapshot Type:**
    *   **RSYNC:** This method uses `rsync` to create full system backups that are incrementally updated. It is filesystem-agnostic and works well on any Linux filesystem (Ext4, XFS, etc.). While efficient, it consumes more disk space than Btrfs snapshots for multiple versions.
    *   **BTRFS:** This method leverages the native snapshot capabilities of the Btrfs filesystem. [Btrfs snapshots](https://wiki.archlinux.org/title/Btrfs#Snapshots) are copy-on-write, meaning they initially take up very little space and only store changes from the original. This is highly efficient and recommended if your root filesystem is Btrfs.
3.  **Select Snapshot Location:**
    *   Choose a separate partition or external drive. **Do NOT save snapshots on the same partition as your root filesystem (for rsync type).**
    *   For Btrfs, you typically select the root Btrfs filesystem, and Timeshift will manage the snapshots as subvolumes.
4.  **Schedule Snapshots:** Configure the frequency (e.g., daily, weekly) and retention policy.
5.  **Create First Snapshot:** Once configured, create your first snapshot immediately.

---

## Step 3: Command Line Usage (Optional)

While Timeshift is GUI-focused, you can also manage snapshots from the command line after initial setup.

```bash
timeshift --help             # Display help and available commands
timeshift --check            # Check system for issues before snapshot
timeshift --create --comments "Manual snapshot" --tags D # Create a snapshot
timeshift --list             # List existing snapshots
timeshift --restore          # Restore from a snapshot (requires reboot into live environment)
```

**Note on restoring:** For restoring a system snapshot, it is often safer and recommended to boot from a live Arch Linux USB environment, install Timeshift there, and then use it to restore the snapshot. This ensures no system files are in use during the restore process.

---

**SUCCESS:** Timeshift installed and configured for system snapshots. Regularly create and verify your snapshots.

**Next:** Consider configuring personal data backups (e.g., using `rsync` or cloud sync services) as Timeshift is primarily for system files.

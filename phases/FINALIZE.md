# Phase 09: Finalize Installation

**Purpose:** Exit chroot, unmount partitions, and reboot the system. This final phase prepares your system for its first boot into the newly installed Arch Linux environment. For a detailed guide on the finalization steps, refer to the [ArchWiki Installation Guide#Reboot](https://wiki.archlinux.org/title/Installation_guide#Reboot).

**Prerequisites:**
- All configuration completed
- System ready for first boot

**Time Estimate:** 5 minutes
**Difficulty:** ⭐ Easy

**Steps:**

1. **[Exit Chroot and Reboot](../steps/exit-chroot.md)**
   - Exit chroot environment
   - Unmount all partitions
   - Close encrypted volumes (if used)
   - Reboot system

## Verification

- [ ] Exited chroot successfully
- [ ] All partitions unmounted
- [ ] Encrypted volumes closed (if used)
- [ ] System ready to reboot

## After Reboot

- GRUB menu appears
- Select Arch Linux
- Enter LUKS passphrase (if not using keyfile)
- Login with created user
- Continue with post-installation configuration (if needed)

---

## Navigation

**Previous:**
- **[← Phase 08: Hardware](HARDWARE.md)** (if you configured hardware)
- **[← Phase 07: Security](SECURITY.md)** (if you skipped hardware)

**Next:** This is the final phase. Installation complete!

---

**Back to:** [Main Index](../INDEX.md) | [Repository Root](../README.md)

---

**INSTALLATION COMPLETE!**

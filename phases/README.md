# Phases Directory

**Purpose:** This directory contains organized installation phases. Each phase groups related installation steps into logical units, making it easier to follow a complete installation procedure.

---

## What are Phases?

Phases are **organized installation groups** that:
- ✅ Group related steps together
- ✅ Provide logical installation flow
- ✅ Link to detailed modules/steps
- ✅ Guide users through complete installation
- ✅ Recommended for structured installation

---

## Phase-Based Installation

Instead of picking individual modules, phases provide a **structured approach** to installation:

1. **Preparation** - USB creation, Windows installation, disk formatting
2. **Disk Setup** - Partitioning, encryption, filesystem
3. **System Install** - Base system installation, chroot
4. **Basic Config** - Locale, user creation, sudo
5. **Bootloader** - GRUB installation, LUKS auto-unlock
6. **Network** - NetworkManager, WiFi, Bluetooth
7. **Audio** - PipeWire audio server
8. **Security** - Firewall, SSH protection
9. **Hardware** - Touchpad, webcam, IR camera, fingerprint
10. **Finalize** - Exit chroot, reboot

---

## Available Phases

### Phase 00: Preparation
**File:** `00-preparation.md`

Covers:
- Creating Arch Linux USB drive
- Installing Windows (for dual boot)
- Formatting disk (optional)

**When to use:** Always - first step of any installation

---

### Phase 01: Disk Setup
**File:** `01-disk-setup.md`

Covers:
- Disk partitioning
- LUKS encryption (optional)
- Btrfs filesystem (optional)
- Mounting partitions

**When to use:** If disk needs setup (new installation)

---

### Phase 02: System Install
**File:** `02-system-install.md`

Covers:
- Base system installation (pacstrap)
- Generating fstab
- Entering chroot

**When to use:** Always - core installation step

---

### Phase 03: Basic Config
**File:** `03-basic-config.md`

Covers:
- Setting locale and timezone
- Creating user account
- Configuring sudo access

**When to use:** Always - basic system configuration

---

### Phase 04: Bootloader
**File:** `04-bootloader.md`

Covers:
- Installing GRUB
- Configuring GRUB for LUKS (if encrypted)
- Setting up auto-unlock (optional)

**When to use:** Always - system won't boot without this

---

### Phase 05: Network
**File:** `05-network.md`

Covers:
- Enabling NetworkManager
- WiFi support
- Bluetooth support

**When to use:** If you need network connectivity

---

### Phase 06: Audio
**File:** `06-audio.md`

Covers:
- Installing PipeWire audio stack

**When to use:** If you need audio support

---

### Phase 07: Security
**File:** `07-security.md`

Covers:
- Configuring UFW firewall
- Installing Fail2ban
- SSH server configuration

**When to use:** Recommended for all installations

---

### Phase 08: Hardware
**File:** `08-hardware.md`

Covers:
- Touchpad configuration
- Webcam configuration
- IR camera (face recognition)
- Fingerprint reader

**When to use:** For laptops with specific hardware

---

### Phase 09: Finalize
**File:** `09-finalize.md`

Covers:
- Exiting chroot
- Unmounting partitions
- Rebooting system

**When to use:** Always - final step before first boot

---

## How to Use Phases

1. **Start with Phase 00:** Always begin with Preparation
2. **Follow sequentially:** Phases build on each other
3. **Skip optional phases:** Not all phases are required
4. **Check prerequisites:** Each phase lists what's needed
5. **Follow links:** Phases link to detailed modules/steps

---

## Phase vs Module vs Step

**Phases** (this directory):
- Organized groups of related tasks
- Logical installation flow
- Links to modules/steps
- Recommended approach

**Modules** (`../modules/`):
- Detailed guides with explanations
- Troubleshooting included
- Educational content

**Steps** (`../steps/`):
- Commands only
- Quick reference
- Minimal text

---

## Quick Reference

- **Installation Flows:** See `../wiki/installation-flows.md` for pre-built scenarios
- **Installation Scenarios:** See `../installation-scenarios.md` for different use cases
- **Complete Guide:** See `../wiki/complete-installation.md` for all phases in one document
- **Module Index:** See `../MODULE_index.md` for all available modules

---

**Back to:** [Repository Root](../README.md)

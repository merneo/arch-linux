# Wiki Directory

**Purpose:** This directory contains comprehensive documentation, guides, and navigation pages for the Arch Linux Modular Installation System.

---

## What is in Wiki?

The wiki directory provides:
- ✅ Entry points and navigation
- ✅ Complete installation guides
- ✅ Installation flow scenarios
- ✅ Post-installation configuration
- ✅ User-friendly documentation

---

## Wiki Files

### HOME.md
**Purpose:** Main entry point and navigation menu

**Contains:**
- Quick start links
- Installation paths
- Links to all major sections
- Overview of the system

**When to use:** Start here to understand the repository structure

---

### pre-installation.md
**Purpose:** Pre-installation steps and preparation

**Contains:**
- USB creation instructions
- Windows installation (dual boot)
- Disk formatting
- Preparation checklist

**When to use:** Before starting Arch Linux installation

---

### core-installation.md
**Purpose:** Core system installation guide

**Contains:**
- Base system installation (pacstrap)
- Chroot setup
- Essential configuration
- Default view for installation

**When to use:** Main installation steps

---

### post-installation.md
**Purpose:** Post-installation configuration guide

**Contains:**
- All post-installation modules
- Desktop environments
- Window managers
- Applications and tools
- GPU drivers
- Security configuration

**When to use:** After first boot into new system

---

### complete-installation.md
**Purpose:** Complete installation guide with all phases

**Contains:**
- All 10 phases in sequence
- Complete installation path
- Links to all modules
- Comprehensive guide

**When to use:** For complete installation walkthrough

---

### installation-flows.md
**Purpose:** Pre-built installation scenarios

**Contains:**
- Encrypted single boot flow
- Non-encrypted single boot flow
- Dual boot flow
- Minimal installation flow
- Desktop installation flow

**When to use:** To find a scenario matching your needs

---

## Navigation Flow

```
HOME.md
  ├── pre-installation.md (if needed)
  ├── core-installation.md (main installation)
  │   └── post-installation.md (after first boot)
  ├── complete-installation.md (all-in-one guide)
  └── installation-flows.md (pre-built scenarios)
```

---

## How to Use Wiki

1. **New users:** Start with `HOME.md` → `INSTALLATION-FLOWS.md`
2. **Experienced users:** Use `COMPLETE-INSTALLATION.md` for reference
3. **Custom installation:** Use `HOME.md` → individual modules
4. **Troubleshooting:** Check specific module pages

---

## Wiki vs Other Directories

**Wiki** (this directory):
- User-facing documentation
- Navigation and guides
- Complete workflows
- Entry points

**Modules** (`../modules/`):
- Detailed technical guides
- Individual module documentation
- Step-by-step instructions

**Steps** (`../steps/`):
- Command-only references
- Quick execution guides

**Phases** (`../phases/`):
- Organized installation groups
- Logical flow structure

---

## Quick Reference

- **Repository Root:** See `../README.md` for overview
- **Module Index:** See `../MODULE_index.md` for all modules
- **Step Index:** See `../STEP_index.md` for all steps
- **Installation Scenarios:** See `../installation-scenarios.md`

---

**Back to:** [Repository Root](../README.md) | [Home](HOME.md)

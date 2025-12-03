# Time Estimates

**Purpose:** Realistic time estimates for each installation phase and complete scenarios to help you plan your installation.

**Note:** Times are estimates and vary based on:
- Internet connection speed
- Hardware performance
- Your experience level
- Complexity of your configuration

---

## Phase-by-Phase Time Estimates

### Phase 00: Preparation
**Time:** 30-60 minutes  
**Difficulty:** ⭐ Easy

- Create Arch Linux USB: 10-20 minutes
- Install Windows (if dual boot): 20-40 minutes
- Format disk (if needed): 5-10 minutes

**Factors:**
- USB creation speed (depends on USB drive speed)
- Windows installation (depends on hardware)
- Disk formatting (depends on disk size)

---

### Phase 01: Disk Setup
**Time:** 15-30 minutes  
**Difficulty:** ⭐⭐ Intermediate

- Disk partitioning: 5-10 minutes
- LUKS encryption: 5-10 minutes (if using)
- Btrfs filesystem: 5-10 minutes
- Mount partitions: 2-5 minutes

**Factors:**
- Disk size (larger = longer formatting)
- Encryption (adds 5-10 minutes)
- Experience with partitioning

---

### Phase 02: System Install
**Time:** 10-20 minutes  
**Difficulty:** ⭐ Easy

- Core system installation: 10-20 minutes
- Enter chroot: 1 minute

**Factors:**
- **Internet speed** (most important!)
  - Fast (100+ Mbps): 10-15 minutes
  - Medium (20-100 Mbps): 15-25 minutes
  - Slow (<20 Mbps): 25-40 minutes
- Disk speed (SSD vs HDD)

---

### Phase 03: Basic Configuration
**Time:** 10-15 minutes  
**Difficulty:** ⭐ Easy

- Locale and timezone: 3-5 minutes
- User creation: 5-10 minutes
- Root password: 1 minute

**Factors:**
- Familiarity with text editors
- Time to think about user names

---

### Phase 04: Bootloader
**Time:** 10-15 minutes  
**Difficulty:** ⭐⭐ Intermediate

- GRUB installation: 5-10 minutes
- LUKS auto-unlock (if using): 5-10 minutes

**Factors:**
- LUKS configuration complexity
- Dual boot setup (adds 2-5 minutes)

---

### Phase 05: Network Configuration
**Time:** 5-10 minutes  
**Difficulty:** ⭐ Easy

- NetworkManager: 2-3 minutes
- WiFi (if using): 3-5 minutes
- Bluetooth (if using): 2-3 minutes

**Factors:**
- WiFi configuration complexity
- Hardware compatibility

---

### Phase 06: Audio Configuration
**Time:** 5-10 minutes  
**Difficulty:** ⭐ Easy

- PipeWire installation: 3-5 minutes
- Service configuration: 2-5 minutes

**Factors:**
- Usually straightforward

---

### Phase 07: Security Configuration
**Time:** 15-20 minutes  
**Difficulty:** ⭐⭐ Intermediate

- SSH server: 5-10 minutes
- UFW firewall: 5-10 minutes
- Fail2ban: 5-10 minutes

**Factors:**
- Security requirements
- Configuration complexity

---

### Phase 08: Hardware Configuration (Laptops)
**Time:** 10-20 minutes  
**Difficulty:** ⭐⭐ Intermediate

- Touchpad: 3-5 minutes
- Webcam: 3-5 minutes
- IR camera (if present): 5-10 minutes
- Fingerprint (if present): 5-10 minutes

**Factors:**
- Number of hardware components
- Hardware compatibility

---

### Phase 09: Finalize
**Time:** 5 minutes  
**Difficulty:** ⭐ Easy

- Exit chroot: 1 minute
- Unmount partitions: 1 minute
- Reboot: 2-3 minutes

**Factors:**
- Usually quick

---

## Complete Installation Time Estimates

### Minimal Installation
**Total Time:** 1-2 hours

**Includes:**
- Phase 00: Preparation (30-60 min)
- Phase 01: Disk Setup (15-30 min)
- Phase 02: System Install (10-20 min)
- Phase 03: Basic Config (10-15 min)
- Phase 04: Bootloader (10-15 min)
- Phase 05: Network (5-10 min)
- Phase 09: Finalize (5 min)

**After First Boot:**
- Basic setup: 30-60 minutes

---

### Standard Installation
**Total Time:** 2-4 hours

**Includes:**
- All minimal phases
- Phase 06: Audio (5-10 min)
- Phase 07: Security (15-20 min)
- Desktop environment: 30-60 minutes
- Essential applications: 30-60 minutes

**After First Boot:**
- Desktop setup: 1-2 hours

---

### Full Installation (Encrypted + Desktop)
**Total Time:** 3-6 hours

**Includes:**
- All standard phases
- LUKS encryption: +15-30 minutes
- LUKS auto-unlock: +10-15 minutes
- GPU drivers: 20-40 minutes
- Full desktop environment: 30-60 minutes
- All applications: 1-2 hours

**After First Boot:**
- Complete setup: 2-3 hours

---

### Dual Boot Installation
**Total Time:** 3-5 hours

**Includes:**
- Windows installation: +20-40 minutes
- Windows configuration: +10-20 minutes
- Dual boot partitioning: +5-10 minutes
- GRUB os-prober: +2-5 minutes
- Standard Arch installation

**After First Boot:**
- Dual boot verification: 15-30 minutes

---

### Laptop Installation (Full)
**Total Time:** 4-7 hours

**Includes:**
- All full installation phases
- Phase 08: Hardware (10-20 min)
- WiFi configuration: +5-10 minutes
- Touchpad/webcam: +10-15 minutes
- Biometrics (if present): +10-20 minutes

**After First Boot:**
- Hardware testing: 30-60 minutes

---

## Time by Experience Level

### Beginner (First Time)
**Multiplier:** 1.5-2x base time

- More time reading instructions
- More time troubleshooting
- Learning curve

**Example:** Minimal installation = 1.5-3 hours

---

### Intermediate (Some Linux Experience)
**Multiplier:** 1x base time

- Can follow instructions efficiently
- Some troubleshooting knowledge

**Example:** Minimal installation = 1-2 hours

---

### Advanced (Experienced Linux User)
**Multiplier:** 0.7-0.8x base time

- Fast execution
- Know what to skip/modify
- Minimal troubleshooting

**Example:** Minimal installation = 45-90 minutes

---

## Factors Affecting Installation Time

### Internet Speed
- **Fast (100+ Mbps):** Base time
- **Medium (20-100 Mbps):** +20-30% time
- **Slow (<20 Mbps):** +50-100% time

**Impact:** Most significant on Phase 02 (System Install)

---

### Hardware Performance
- **SSD:** Base time
- **HDD:** +20-30% time
- **Old hardware:** +30-50% time

**Impact:** Disk operations, boot times

---

### Configuration Complexity
- **Simple (no encryption, basic setup):** Base time
- **Medium (encryption, standard desktop):** +30-50% time
- **Complex (full encryption, multiple features):** +50-100% time

---

### Troubleshooting
- **No issues:** Base time
- **Minor issues:** +15-30 minutes
- **Major issues:** +1-3 hours

**Tip:** Follow [INSTALLATION_CHECKLIST.md](INSTALLATION_CHECKLIST.md) to minimize errors!

---

## Realistic Planning

### Recommended Time Allocation

**For First-Time Installation:**
- Allocate **full day** (6-8 hours)
- Include breaks
- Have backup plan (can continue next day)

**For Experienced Users:**
- Allocate **half day** (3-4 hours)
- Can complete in one session

---

### Best Time to Install

**Recommended:**
- Weekend with no time pressure
- Good internet connection available
- Backup of important data completed
- All prerequisites ready

**Avoid:**
- Rushed situations
- Unstable internet
- Without data backup

---

## Post-Installation Time

### First Boot Setup
**Time:** 30-60 minutes

- Network configuration
- System updates
- GPU drivers
- Desktop environment

---

### Complete System Setup
**Time:** 2-4 hours

- All applications
- System customization
- Performance tuning
- Backup setup

---

## Summary Table

| Installation Type | Base Time | Beginner | Intermediate | Advanced |
|-------------------|-----------|----------|--------------|----------|
| Minimal | 1-2h | 1.5-3h | 1-2h | 45-90min |
| Standard | 2-4h | 3-6h | 2-4h | 1.5-3h |
| Full | 3-6h | 4.5-9h | 3-6h | 2-4.5h |
| Dual Boot | 3-5h | 4.5-7.5h | 3-5h | 2-4h |
| Laptop Full | 4-7h | 6-10.5h | 4-7h | 3-5.5h |

---

**Tip:** Use [INSTALLATION_CHECKLIST.md](INSTALLATION_CHECKLIST.md) to track progress and stay on schedule!

---

**Back to:** [Main Index](INDEX.md) | [Repository Root](README.md)

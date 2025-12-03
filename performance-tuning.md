# Performance Tuning Guide

**Purpose:** Tips and techniques to optimize your Arch Linux system performance after installation.

**Quick Links:**
- [Post-Installation Checklist](post-install-checklist.md) - Complete setup
- [Troubleshooting](troubleshooting.md) - If issues occur

---

## Boot Time Optimization

### Reduce Boot Time

1. **Disable unnecessary services:**
   ```bash
   systemctl list-unit-files --type=service --state=enabled
   systemctl disable <unnecessary-service>
   ```

2. **Optimize GRUB timeout:**
   ```bash
   sudo nano /etc/default/grub
   # Set: GRUB_TIMEOUT=2
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

3. **Use systemd-analyze:**
   ```bash
   systemd-analyze  # Check boot time
   systemd-analyze blame  # See what's slow
   ```

---

## SSD Optimization

### Enable TRIM (for SSDs)

1. **Check if TRIM is enabled:**
   ```bash
   systemctl status fstrim.timer
   ```

2. **Enable TRIM:**
   ```bash
   sudo systemctl enable fstrim.timer
   ```

3. **For Btrfs:**
   ```bash
   # Add to /etc/fstab: discard mount option
   # Or use: sudo btrfs filesystem defrag -r -v /
   ```

---

## Memory Optimization

### Swap Configuration

1. **Check swap usage:**
   ```bash
   free -h
   swapon --show
   ```

2. **Optimize swappiness:**
   ```bash
   # For desktop: lower swappiness
   echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
   ```

3. **For systems with enough RAM:**
   ```bash
   # Can disable swap if >16GB RAM
   sudo swapoff -a
   ```

---

## CPU Optimization

### CPU Governor

1. **Check current governor:**
   ```bash
   cpupower frequency-info
   ```

2. **Set performance governor:**
   ```bash
   sudo cpupower frequency-set -g performance
   ```

3. **For laptops (battery saving):**
   ```bash
   sudo cpupower frequency-set -g powersave
   ```

---

## Disk I/O Optimization

### I/O Scheduler

1. **Check current scheduler:**
   ```bash
   cat /sys/block/sdX/queue/scheduler
   ```

2. **Set scheduler (for SSDs):**
   ```bash
   # Use mq-deadline or none for NVMe
   echo mq-deadline | sudo tee /sys/block/sdX/queue/scheduler
   ```

3. **Make permanent:**
   ```bash
   # Add to /etc/udev/rules.d/60-ssd-scheduler.rules
   ACTION=="add|change", KERNEL=="sd[a-z]", ATTR{queue/rotational}=="0", ATTR{queue/scheduler}="mq-deadline"
   ```

---

## Btrfs Optimization

### Compression

1. **Enable compression:**
   ```bash
   # Add to /etc/fstab: compress=zstd
   # Or remount: sudo mount -o remount,compress=zstd /
   ```

2. **Benefits:**
   - Saves disk space
   - Can improve read performance
   - Minimal CPU overhead

### Defragmentation

1. **For Btrfs:**
   ```bash
   # Defragment (if needed, rarely)
   sudo btrfs filesystem defrag -r -v /
   ```

**Note:** Usually not needed with Btrfs.

---

## Network Optimization

### TCP Settings

1. **Optimize TCP buffers:**
   ```bash
   # Add to /etc/sysctl.conf:
   net.core.rmem_max = 16777216
   net.core.wmem_max = 16777216
   net.ipv4.tcp_rmem = 4096 87380 16777216
   net.ipv4.tcp_wmem = 4096 65536 16777216
   ```

2. **Apply:**
   ```bash
   sudo sysctl -p
   ```

---

## Graphics Performance

### NVIDIA

1. **Enable performance mode:**
   ```bash
   sudo nvidia-smi -pm 1
   sudo nvidia-smi -pl <power_limit>
   ```

2. **Check GPU usage:**
   ```bash
   nvidia-smi
   watch -n 1 nvidia-smi
   ```

### AMD

1. **Check GPU performance:**
   ```bash
   radeontop  # Install: pacman -S radeontop
   ```

---

## Application Performance

### Reduce Startup Applications

1. **Check autostart:**
   ```bash
   # GNOME
   gnome-session-properties
   
   # KDE
   systemsettings -> Startup and Shutdown
   ```

2. **Disable unnecessary autostart:**
   - Remove unused applications
   - Delay startup if possible

---

## System Monitoring

### Tools

1. **htop:** Better top
   ```bash
   pacman -S htop
   htop
   ```

2. **iotop:** Disk I/O monitoring
   ```bash
   pacman -S iotop
   sudo iotop
   ```

3. **systemd-analyze:** Boot analysis
   ```bash
   systemd-analyze
   systemd-analyze blame
   systemd-analyze critical-chain
   ```

---

## Package Management

### Optimize Pacman

1. **Parallel downloads:**
   ```bash
   # In /etc/pacman.conf:
   # Uncomment: ParallelDownloads = 5
   ```

2. **Clean package cache:**
   ```bash
   sudo pacman -Sc  # Remove unused
   sudo pacman -Scc  # Remove all (careful!)
   ```

---

## Desktop Environment Optimization

### GNOME

1. **Disable extensions:**
   - Remove unused extensions
   - Disable animations if needed

2. **Reduce visual effects:**
   ```bash
   gsettings set org.gnome.desktop.interface enable-animations false
   ```

### KDE Plasma

1. **Disable effects:**
   - System Settings -> Workspace Behavior -> Desktop Effects
   - Disable unnecessary effects

2. **Reduce animations:**
   - System Settings -> Workspace Behavior -> General Behavior

---

## Summary

### Quick Wins
- ✅ Enable TRIM for SSDs
- ✅ Optimize swappiness
- ✅ Disable unnecessary services
- ✅ Clean package cache
- ✅ Reduce GRUB timeout

### Advanced
- ⚙️ CPU governor tuning
- ⚙️ I/O scheduler optimization
- ⚙️ Btrfs compression
- ⚙️ Network tuning

---

**Remember:** Measure before and after to verify improvements!

---

**Back to:** [Main Index](index.md) | [Repository Root](README.md)

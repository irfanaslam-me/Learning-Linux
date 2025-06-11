# Day 2 – Linux Filesystem Structure

## 🎯 Objectives
- Learn the directory structure in Linux
- Understand where configuration files, user data, and logs are stored

---

## 📁 What is Filesystem Hierarchy Standard (FHS)?

The **FHS** defines the structure and purpose of directories on Linux. It ensures that all Linux distros follow a predictable layout.

---

## 🗂️ Key Directories in Linux

| Directory | Purpose |
|----------|---------|
| `/`      | Root directory – everything starts here |
| `/bin`   | Essential user binaries (e.g. `ls`, `cp`) |
| `/sbin`  | System binaries (e.g. `ifconfig`, `shutdown`) |
| `/etc`   | System configuration files |
| `/home`  | User home directories |
| `/root`  | Home directory for root user |
| `/tmp`   | Temporary files |
| `/var`   | Variable data (e.g., logs, mail, spool) |
| `/usr`   | User-installed software and libraries |
| `/boot`  | Boot loader and kernel files |
| `/lib`, `/lib64` | Shared libraries needed by programs |
| `/proc`  | Virtual filesystem for kernel and process info |
| `/dev`   | Device files (e.g., `/dev/sda`, `/dev/null`) |
| `/mnt` and `/media` | Mount points for external devices |

---

## 🔍 Common Commands

```bash
ls /          # List top-level directories
cd /etc       # Change to /etc
ls -l /bin    # List binaries with details
df -h         # Show disk usage by mounted filesystems
du -sh /home  # Show total size of /home

# 30-Day Linux System Administration Course Outline

## Week 1: Linux Basics & Environment

### [Day 1 – Introduction to Linux & Distributions](./Day01_Introduction/)
- History and philosophy of Linux
- Popular distributions: Ubuntu, CentOS, Debian, Red Hat
- Installation on VirtualBox/VMware or Cloud

### [Day 2 – Linux Filesystem Structure](./Day02_Filesystem_Structure/)
- FHS (Filesystem Hierarchy Standard)
- Key directories: `/etc`, `/var`, `/home`, `/root`, `/boot`, `/proc`

### Day 3 – Basic Shell Commands
- Navigating directories
- Creating, copying, moving, deleting files
- Using `man`, `echo`, `cat`, `touch`, `clear`, etc.

### Day 4 – Users & Groups Management
- Creating/deleting users and groups
- Password policies
- Commands: `usermod`, `passwd`, `groupadd`, `id`

### Day 5 – File Permissions and Ownership
- `chmod`, `chown`, `chgrp`
- Understanding r/w/x for users, groups, others
- `umask` and default permissions

### Day 6 – Package Management
- Tools: `apt`, `yum`, `dnf`, `zypper`
- Installing/removing software
- Updating and searching packages

### Day 7 – Practice & Mini Project
- Setup a multi-user environment with restricted access
- Install and configure 3 packages

---

## Week 2: Shell Scripting & Process Management

### [Day 8 – Shell & Bash Basics](./Day08_Shell_Basics/)
- Shell types
- Bash environment, aliases, variables

### [Day 9 – Writing Basic Shell Scripts](./Day09_Shell_Scripting/)
- Shebang, variables, `echo`, `read`, conditions, loops

### [Day 10 – Scheduled Tasks (Cron Jobs)](./Day10_Cron_Jobs/)
- `crontab` syntax and examples
- `at`, `anacron`
- Logging cron jobs

### Day 11 – Process & Job Management
- Foreground/background processes
- Commands: `ps`, `top`, `kill`, `nice`, `htop`

### Day 12 – System Boot Process
- BIOS to bootloader (GRUB)
- Systemd services
- Commands: `systemctl`, `journalctl`

### Day 13 – Logging & Log Rotation
- Location of logs in `/var/log`
- Tools: `rsyslog`, `logrotate`

### Day 14 – Script Challenge
- Write a backup script with logging
- Setup cron to automate it

---

## Week 3: Networking & Storage

### Day 15 – Networking Basics
- IP, subnet, DNS, gateway concepts
- Tools: `ip`, `ifconfig`, `netstat`, `ping`, `dig`

### Day 16 – Network Configuration
- Static & DHCP setup
- Hosts file, DNS resolution
- Firewall intro: `ufw`, `firewalld`

### Day 17 – Disk Management
- Tools: `lsblk`, `df`, `du`, `mount`, `umount`
- Partitioning with `fdisk`, `parted`

### Day 18 – Filesystems & Swap
- ext4, xfs, btrfs basics
- Creating/mounting filesystems
- Swap setup and usage

### Day 19 – LVM (Logical Volume Management)
- Creating PV, VG, LV
- Extending/shrinking volumes
- Snapshots

### Day 20 – File Sharing & NFS
- NFS server/client setup
- Permissions, exports file configuration

### Day 21 – Samba & FTP Services
- Sharing with Windows via Samba
- FTP server with `vsftpd`

---

## Week 4: Services, Security, and Final Project

### Day 22 – Web Server Basics
- Installing Apache/Nginx
- Serving static content
- Logs, ports, index file

### Day 23 – Database Server Setup
- Install MySQL/MariaDB/PostgreSQL
- Basic security (`mysql_secure_installation`)
- User management and backup

### Day 24 – SSH & Remote Access
- Key-based login
- SSH hardening
- Port forwarding and tunneling

### Day 25 – Linux Firewall & SELinux
- Tools: `iptables`, `firewalld`, zones
- SELinux modes, booleans, troubleshooting

### Day 26 – System Monitoring & Performance
- Tools: `vmstat`, `iostat`, `free`, `iotop`
- Additional tools: `uptime`, `sar`, `dstat`, `sysstat`

### Day 27 – Backup & Recovery
- Tools: `rsync`, `tar`, `gzip`
- Full/Incremental backup and restore

### Day 28 – Automation with Ansible (Intro)
- YAML basics
- Writing a simple Ansible playbook
- Configuring a remote Linux server

### Day 29 – Final Review
- Recap of all topics
- Practice MCQs or test
- Mock troubleshooting tasks

### Day 30 – Final Project & Presentation
- Deploy a secure web server with DB and backup
- User documentation and service status reports
- Present setup, automation, and monitoring

---

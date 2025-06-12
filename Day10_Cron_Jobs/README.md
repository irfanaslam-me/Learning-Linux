# 📅 Day 10 – Scheduled Tasks (Cron Jobs)

## 🎯 Objectives

- Understand what cron and crontab are
- Learn how to schedule recurring tasks
- Know how to edit and manage cron jobs
- Use correct crontab syntax
- Automate common system maintenance tasks

---

## ⏰ What is Cron?

**Cron** is a Linux utility used to **schedule scripts or commands** to run automatically at specified times and dates.

It is useful for:

- Running backups
- Cleaning logs
- Sending reports
- Automating system updates

---

## 📘 What is Crontab?

**Crontab** stands for **Cron Table**. It’s a file containing a list of commands to be run at scheduled times.

Each user (including root) can have their **own crontab**.

---

## 🔧 Crontab Commands

| Command                  | Description                        |
|--------------------------|------------------------------------|
| `crontab -e`             | Edit the current user's crontab    |
| `crontab -l`             | List current user's cron jobs      |
| `crontab -r`             | Remove the current user's crontab  |
| `crontab -u username -e` | Edit another user's crontab (root) |

---

## 🕰 Crontab Syntax

```bash
* * * * * /path/to/command arg1 arg2
│ │ │ │ │
│ │ │ │ └─ Day of week (0–7) (Sunday = 0 or 7)
│ │ │ └─── Month (1–12)
│ │ └───── Day of month (1–31)
│ └─────── Hour (0–23)
└───────── Minute (0–59)

### Example
```bash
30 14 * * 1 /home/user/backup.sh
```
> 🡺 Runs every Monday at 2:30 PM

## 🧪 Common Cron Time Examples

| Schedule                | Syntax       | Description                      |
| ----------------------- | ------------ | -------------------------------- |
| Every minute            | `* * * * *`  | Every minute                     |
| Every hour              | `0 * * * *`  | Top of every hour                |
| Daily at 6AM            | `0 6 * * *`  | Every day at 6:00 AM             |
| Weekly on Sunday at 3PM | `0 15 * * 0` | Every Sunday at 3:00 PM          |
| Monthly on 1st, 12 AM   | `0 0 1 * *`  | First of every month at midnight |
| Yearly (Jan 1)          | `0 0 1 1 *`  | Once a year on Jan 1 at 12AM     |

## 📁 Location of Cron Jobs

| Cron Type   | Location                | Description                   |
| ----------- | ----------------------- | ----------------------------- |
| User cron   | `crontab -e`            | Per user                      |
| System cron | `/etc/crontab`          | System-wide cron              |
| Directory   | `/etc/cron.daily/` etc. | Auto-run scripts (no crontab) |

## 📄 Sample Crontab Entries

```bash
# Run a backup every day at 2 AM
0 2 * * * /home/user/scripts/backup.sh

# Clear temp files every Sunday at midnight
0 0 * * 0 /home/user/scripts/clean_temp.sh

# Send a weekly email report
30 8 * * 1 /home/user/scripts/send_report.sh

```

## 🔁 Special Strings in Cron
You can use shortcuts instead of full time fields:

| String                | Equivalent  | Description             |
| --------------------- | ----------- | ----------------------- |
| `@reboot`             | N/A         | Run once at system boot |
| `@hourly`             | `0 * * * *` | Run every hour          |
| `@daily`              | `0 0 * * *` | Run once a day          |
| `@weekly`             | `0 0 * * 0` | Run once a week         |
| `@monthly`            | `0 0 1 * *` | Run once a month        |
| `@yearly`/`@annually` | `0 0 1 1 *` | Run once a year         |


## 📧 Logging and Output
- By default, cron emails output to the user.

- To log output to a file:

```bash

0 2 * * * /path/to/command.sh >> /var/log/cron.log 2>&1
```
- Disable output:

```bash

0 2 * * * /path/to/command.sh > /dev/null 2>&1
```

## 🧪 Practice Tasks

1. Schedule a script to display the date every 10 minutes.
2. Run a script every Friday at 5 PM to archive logs.
3. Use @reboot to start a Python server on boot.
4. Log the output of a script that checks disk usage.

## ⚠️ Troubleshooting Cron Jobs
- Make sure the script is executable: chmod +x script.sh
- Use absolute paths inside scripts.
- Use full paths to commands (e.g., /usr/bin/python3).
- Check logs:

```bash
grep CRON /var/log/syslog         # Debian/Ubuntu
grep CRON /var/log/cron           # CentOS/RHEL
```

## 🛠️ Example Script
```bash
#!/bin/bash
# disk_report.sh

df -h > /home/user/reports/disk_report.txt
```

Schedule with crontab:

```bash
0 9 * * * /home/user/scripts/disk_report.sh
```

## 📚 Resources
- [Crontab Guru (Crontab Helper)](https://crontab.guru/)
- [man crontab](https://#)
- [Linuxize – Cron Tutorial](https://linuxize.com/post/scheduling-cron-jobs-with-crontab/)



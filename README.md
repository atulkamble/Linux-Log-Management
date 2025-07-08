log management and analysis are crucial parts of Linux system administration, troubleshooting, and security monitoring. Here’s a well-organized, practical list of Linux log management and analysis commands you can use on most distributions:

---

## 📂 Linux Log Locations (by default)

| Log Type            | File Path                                |
| :------------------ | :--------------------------------------- |
| System Logs         | `/var/log/messages`                      |
| Authentication Logs | `/var/log/auth.log` or `/var/log/secure` |
| Kernel Logs         | `/var/log/kern.log`                      |
| Boot Logs           | `/var/log/boot.log`                      |
| Cron Logs           | `/var/log/cron`                          |
| Application Logs    | Typically in `/var/log/<app_name>/`      |

---

## 📖 Log Viewing Commands

| Command                     | Purpose                                                   |
| :-------------------------- | :-------------------------------------------------------- |
| `cat /var/log/syslog`       | View entire log file (not recommended for large logs)     |
| `less /var/log/syslog`      | View with paging, supports search                         |
| `more /var/log/messages`    | Basic pager viewer                                        |
| `tail /var/log/messages`    | View last 10 lines                                        |
| `tail -f /var/log/messages` | Live follow log                                           |
| `head /var/log/messages`    | View first 10 lines                                       |
| `dmesg`                     | View kernel ring buffer (boot-time and hardware messages) |

---

## 📊 Log Filtering and Analysis Commands

| Command                              | Purpose                                                |                          |
| :----------------------------------- | :----------------------------------------------------- | ------------------------ |
| `grep "error" /var/log/syslog`       | Search for "error" entries                             |                          |
| `grep -i "fail" /var/log/auth.log`   | Case-insensitive search                                |                          |
| `grep -A 5 "ERROR" /var/log/app.log` | Show 5 lines **after** each match                      |                          |
| `grep -B 5 "ERROR" /var/log/app.log` | Show 5 lines **before** each match                     |                          |
| `awk '{print $5}' /var/log/syslog`   | Extract the 5th column (assuming space-separated logs) |                          |
| `cut -d' ' -f5 /var/log/syslog`      | Cut out 5th field (space-separated)                    |                          |
| \`sort /var/log/syslog               | uniq -c\`                                              | Count unique log entries |
| `wc -l /var/log/syslog`              | Count lines in a log file                              |                          |

---

## 📑 Log Rotation Management

| Command                            | Purpose                                       |
| :--------------------------------- | :-------------------------------------------- |
| `cat /etc/logrotate.conf`          | Check global logrotate config                 |
| `ls /etc/logrotate.d/`             | View individual app/service logrotate configs |
| `logrotate -d /etc/logrotate.conf` | Debug log rotation configuration              |
| `logrotate -f /etc/logrotate.conf` | Force log rotation                            |

---

## 📈 Real-Time Log Monitoring Tools

| Command                                       | Purpose                                         |
| :-------------------------------------------- | :---------------------------------------------- |
| `tail -f /var/log/messages`                   | Follow log file in real time                    |
| `multitail /var/log/syslog /var/log/auth.log` | View multiple logs in one window (if installed) |
| `journalctl -f`                               | Follow systemd logs live                        |
| `watch -n 5 'tail -n 20 /var/log/messages'`   | Refresh log view every 5 seconds                |

---

## 📜 Systemd Log Management (`journalctl`)

| Command                           | Purpose                                  |
| :-------------------------------- | :--------------------------------------- |
| `journalctl`                      | View all systemd logs                    |
| `journalctl -u nginx.service`     | View logs for a specific service         |
| `journalctl --since "2024-07-01"` | Logs since a specific date               |
| `journalctl --since "1 hour ago"` | Logs from the last hour                  |
| `journalctl -p err`               | View logs with priority `err` and higher |
| `journalctl -b`                   | View logs since last boot                |
| `journalctl --disk-usage`         | Check disk space used by logs            |

---

## 📦 Log Compression & Archival

| Command                                | Purpose                    |
| :------------------------------------- | :------------------------- |
| `gzip /var/log/syslog`                 | Compress log file          |
| `gunzip /var/log/syslog.gz`            | Decompress log file        |
| `tar -czvf logs.tar.gz /var/log/*.log` | Archive multiple log files |

---

## 📌 Pro Tips

* Rotate logs periodically to avoid disk space issues.
* Use `journalctl` in `systemd`-based distros like RHEL 7+, CentOS 7+, Ubuntu 16+.
* Archive old logs to a separate location (e.g., `/var/log/archive/`).
* Monitor logs using tools like **Logwatch**, **GoAccess**, **ELK Stack (Elasticsearch, Logstash, Kibana)**, or **Grafana Loki** for enterprise setups.

---

## ✅ Example: Find SSH Failed Logins

```bash
grep "Failed password" /var/log/auth.log | awk '{print $1, $2, $3, $11}' | sort | uniq -c | sort -nr
```


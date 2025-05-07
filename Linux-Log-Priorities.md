# **Understanding Linux Log Priority Levels: A Detailed Explanation**

Logging is a crucial aspect of Linux system administration, security monitoring, and troubleshooting. Linux systems categorize log messages by priority levels to help administrators quickly identify and respond to important events. Below is a detailed explanation of how log priorities work in Linux.

---

## **1. Why Priority Levels Matter**
Linux generates a massive amount of log data, but not all messages are equally important. Priority levels help:
- **Filter critical issues** (like system crashes) from routine logs (like service startups).
- **Automate monitoring** (e.g., alert on `error` or higher but ignore `info` logs).
- **Improve troubleshooting** by focusing on high-severity logs first.

---

## **2. Breakdown of Priority Levels (From Highest to Lowest Severity)**

### **0 - Emergency (`emerg`)**
- **Meaning**: The system is completely unusable (kernel panics, hardware failures).
- **Example**:  
  ```
  kernel: Out of memory: Kill process 1234 (apache2)
  ```
- **Action Required**: Immediate intervention (server may be crashing).

### **1 - Alert (`alert`)**
- **Meaning**: Immediate action needed (critical hardware/software failure).
- **Example**:  
  ```
  smartd: Disk FAILURE imminent on /dev/sda
  ```
- **Action Required**: Fix immediately to prevent system failure.

### **2 - Critical (`crit`)**
- **Meaning**: Severe errors that disrupt core functionality.
- **Example**:  
  ```
  systemd: Failed to start MySQL database server.
  ```
- **Action Required**: Investigate and resolve quickly.

### **3 - Error (`err`)**
- **Meaning**: Non-critical failures (a service failed but the system is running).
- **Example**:  
  ```
  sshd: Failed password for root from 192.168.1.100
  ```
- **Action Required**: Check for misconfigurations or security issues.

### **4 - Warning (`warning`)**
- **Meaning**: Potential issues that don’t break functionality.
- **Example**:  
  ```
  nginx: 1024 worker_connections are not enough
  ```
- **Action Required**: Monitor and optimize if needed.

### **5 - Notice (`notice`)**
- **Meaning**: Normal but significant events (logins, service restarts).
- **Example**:  
  ```
  systemd: Started Apache Web Server.
  ```
- **Action Required**: Usually just for auditing.

### **6 - Info (`info`)**
- **Meaning**: General operational messages (non-problematic events).
- **Example**:  
  ```
  cron: pam_unix(cron:session): session opened
  ```
- **Action Required**: Typically ignored unless debugging.

### **7 - Debug (`debug`)**
- **Meaning**: Low-level technical details for developers.
- **Example**:  
  ```
  sshd[1234]: debug1: Trying private key: /home/user/.ssh/id_rsa
  ```
- **Action Required**: Only useful for troubleshooting specific issues.

---

## **3. Where Are These Logs Stored?**
### **Traditional syslog Systems (Debian/RHEL)**
- `/var/log/syslog` (general logs)  
- `/var/log/auth.log` (authentication logs)  
- `/var/log/kern.log` (kernel messages)  

### **systemd Journal (Modern Linux)**
- No direct text files (uses binary logs).  
- Accessed via `journalctl`.  

---

## **4. How to Filter Logs by Priority**
### **Using `journalctl` (systemd)**
```bash
# Show only emergency logs (highest severity)
journalctl -p emerg

# Show errors and worse (0-3)
journalctl -p err

# Show warnings and worse (0-4)
journalctl -p warning
```

### **Using `grep` (Traditional Logs)**
```bash
# Find all errors in syslog
grep -i "error" /var/log/syslog

# Find critical, alert, and emergency logs
grep -E "crit|alert|emerg" /var/log/syslog
```

---

## **5. Best Practices for Log Management**
1. **Monitor `error` (3) and higher** for critical issues.
2. **Use log rotation** (`logrotate`) to prevent log files from filling disk space.
3. **Set up alerts** (e.g., with `logwatch` or `fail2ban`) for high-priority logs.
4. **Debug logs (`7`)** should only be enabled temporarily for troubleshooting.


---

## **6. Log Priority Levels Overview**

### **Traditional syslog & systemd journal Priorities**

| Priority Name  | Numeric Value | Description                                                                 | Equivalent in Other System |
|---------------|--------------|-----------------------------------------------------------------------------|---------------------------|
| **emerg**     | 0            | System is unusable (highest severity)                                       | **emerg** (same in journald) |
| **alert**     | 1            | Immediate action required (e.g., hardware failure)                          | **alert** (same) |
| **crit**      | 2            | Critical conditions (e.g., disk failure, system instability)                | **crit** (same) |
| **err**       | 3            | Error conditions (non-critical but require attention)                       | **error** (journald uses "err") |
| **warning**   | 4            | Warning messages (potential issues that don’t disrupt service)              | **warning** (same) |
| **notice**    | 5            | Normal but significant events (e.g., user login, service start)             | **notice** (same) |
| **info**      | 6            | Informational messages (standard operational logs)                          | **info** (same) |
| **debug**     | 7            | Debugging messages (verbose, for developers)                                | **debug** (same) |

---

# **Complete Guide to Log Files in Ubuntu Server**

Ubuntu Server generates various log files stored in `/var/log/` that help monitor system health, security, and application behavior. Below is a detailed breakdown of key log files, their locations, and purposes.

---

## **1. System & Kernel Logs**
| Log File | Purpose | Location |
|----------|---------|----------|
| **syslog** | General system activity logs (most services log here) | `/var/log/syslog` |
| **kern.log** | Kernel messages (hardware, drivers, low-level system events) | `/var/log/kern.log` |
| **boot.log** | Boot process messages (startup services, errors) | `/var/log/boot.log` |
| **dmesg** | Kernel ring buffer (hardware detection, driver errors) | `/var/log/dmesg` (or `dmesg` command) |

---

## **2. Authentication & Security Logs**
| Log File | Purpose | Location |
|----------|---------|----------|
| **auth.log** | Authentication events (logins, sudo, SSH, PAM) | `/var/log/auth.log` |
| **faillog** | Failed login attempts (use `faillog` command to view) | `/var/log/faillog` |
| **btmp** | Bad login attempts (use `lastb` to view) | `/var/log/btmp` |
| **wtmp** | Login history (use `last` to view) | `/var/log/wtmp` |
| **lastlog** | Last login times for all users (use `lastlog` command) | `/var/log/lastlog` |

---

## **3. Package Management Logs**
| Log File | Purpose | Location |
|----------|---------|----------|
| **dpkg.log** | Logs of installed/removed `.deb` packages | `/var/log/dpkg.log` |
| **apt/term.log** | APT package manager operations | `/var/log/apt/term.log` |
| **apt/history.log** | History of `apt` commands (install/remove/upgrade) | `/var/log/apt/history.log` |

---

## **4. Service-Specific Logs**
| Log File | Purpose | Location |
|----------|---------|----------|
| **cron.log** | Scheduled cron jobs (success/failure) | `/var/log/cron.log` |
| **mysql/error.log** | MySQL database errors | `/var/log/mysql/error.log` |
| **postgresql.log** | PostgreSQL database logs | `/var/log/postgresql/` |
| **nginx/** | Nginx web server logs (access & error) | `/var/log/nginx/` |
| **apache2/** | Apache web server logs (access & error) | `/var/log/apache2/` |
| **ufw.log** | Firewall (UFW) logs (blocked/allowed traffic) | `/var/log/ufw.log` |

---

## **5. Application & Miscellaneous Logs**
| Log File | Purpose | Location |
|----------|---------|----------|
| **alternatives.log** | Logs of `update-alternatives` changes (e.g., Java/Python versions) | `/var/log/alternatives.log` |
| **cloud-init.log** | Cloud instance initialization logs (AWS, Azure, etc.) | `/var/log/cloud-init.log` |
| **gpu-manager.log** | GPU driver and configuration logs | `/var/log/gpu-manager.log` |
| **journal/** | `systemd` journal logs (binary format, use `journalctl`) | `/var/log/journal/` |

---

## **6. How to View & Analyze These Logs**
### **Basic Commands**
```bash
# View entire log file
cat /var/log/syslog

# View logs in real-time (follow new entries)
tail -f /var/log/auth.log

# Search for errors in syslog
grep -i "error" /var/log/syslog

# Filter logs by date (e.g., only today's logs)
journalctl --since today
```

### **Advanced Log Analysis**
```bash
# Count failed SSH login attempts
grep "Failed password" /var/log/auth.log | wc -l

# Find the most frequent errors in syslog
grep -i "error" /var/log/syslog | sort | uniq -c | sort -nr

# Check disk space used by logs
du -sh /var/log/*
```

---

## **7. Log Rotation & Maintenance**
Ubuntu uses `logrotate` to prevent logs from consuming too much disk space:
- **Configuration**: `/etc/logrotate.conf`
- **Service-specific rules**: `/etc/logrotate.d/`
  
To manually force log rotation:
```bash
sudo logrotate -f /etc/logrotate.conf
```

---

## **8. Best Practices for Log Management**
1. **Monitor critical logs** (`auth.log`, `syslog`, `nginx/error.log`).
2. **Set up log rotation** to avoid filling `/var/log/`.
3. **Use tools like `logwatch` or `fail2ban`** for automated log analysis.
4. **Forward logs to a central server** (using `rsyslog`) for better security.

---

## **Conclusion**
Ubuntu Server logs provide valuable insights into system health, security, and performance. Knowing where to find these logs and how to analyze them is essential for troubleshooting and maintaining a stable server.

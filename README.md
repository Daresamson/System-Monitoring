# 🛠️ System Monitoring and Log Management  
**Project 2: Step-by-Step Execution**

## 🎯 Objective  
To learn how to monitor system performance and manage logs on a Linux system through hands-on implementation of various tools and configurations.

---

## ✅ Step-by-Step Implementation
![Screenshot (295)](https://github.com/user-attachments/assets/cd31aaa4-a894-4860-a427-f258f5fa036d)
![Screenshot (296)](https://github.com/user-attachments/assets/39cc4e03-0b36-43eb-bf82-597974b363f3)

### 1. **Monitoring System Performance with `htop`**

First, I installed `htop`, a more user-friendly alternative to `top`, for real-time monitoring of CPU, memory, and process usage.

```bash
sudo apt install htop
```

Once installed, I launched `htop`:

```bash
htop
```

This gave me a colorful, interactive view of processes, CPU usage per core, memory utilization, and more—helpful for understanding what's consuming system resources.

---
![Screenshot (297)](https://github.com/user-attachments/assets/c69aba20-8531-4ba7-aaa5-998c789efdd5)

![Screenshot (298)](https://github.com/user-attachments/assets/3c242f7d-fda2-4ed6-906e-3699d317f487)

### 2. **Checking Disk Usage with `df` and `du`**

To keep an eye on how much disk space is used and where, I used two commands:

- To check the overall disk usage in human-readable format:
  ```bash
  df -h
  ```

- To check how much space the `/home` directory was using:
  ```bash
  du -sh /home
  ```

These commands helped me identify which partitions or directories were consuming the most space.

---
![Screenshot (299)](https://github.com/user-attachments/assets/819c4a2a-0420-4059-9c9b-9f2d1047b18a)

### 3. **Setting Up Log Rotation for Custom Logs**

I wanted to ensure that a custom log file (`/var/log/myapp.log`) doesn't grow uncontrollably. I created a logrotate configuration for it:

```bash
sudo nano /etc/logrotate.d/myapp
```

Then I added the following configuration:

```text
/var/log/myapp.log {
    daily
    missingok
    rotate 7
    compress
    delaycompress
    notifempty
    create 0640 root adm
}
```

This setup rotates the log daily, keeps 7 compressed backups, and ensures empty logs are skipped. It also maintains the appropriate file permissions and ownership.

---
![Screenshot (300)](https://github.com/user-attachments/assets/e890ff52-ddf2-4d21-8f0d-0d859b1642ac)

### 4. **Analyzing Logs with `grep`**

To search for specific issues in the logs, I used `grep`. For example, to find all lines containing the word "error" in the system log, I ran:

```bash
grep "error" /var/log/syslog
```

This helped me quickly identify and troubleshoot errors in the system.

---
![Screenshot (301)](https://github.com/user-attachments/assets/a2c58946-8612-4913-9462-093f40ea58aa)
![Screenshot (302)](https://github.com/user-attachments/assets/338cb2df-9d2d-40b5-8ea3-52d995b41968)
![Screenshot (303)](https://github.com/user-attachments/assets/7e4c8223-4393-432c-a9e8-888a6fc29565)

### 5. **Setting Up Disk Usage Alerts with `cron` and `mail`**

To proactively manage disk space, I set up an automatic alert system that emails me if disk usage exceeds 90%.

I edited the root crontab:

```bash
sudo crontab -e
```

And added this line to run every 10 minutes:

```bash
*/10 * * * * df -h | awk '$5+0 > 90 {print $1, $5}' | mail -s "Disk Usage Alert" admin@example.com
```

> Note: I made sure the `mailutils` package was installed to use the `mail` command:
> ```bash
> sudo apt install mailutils
> ```

This means I’ll get an email alert any time a disk partition usage crosses 90%, which is great for avoiding system crashes due to full disks.

---

## 🏁 Conclusion

Through this project, I gained practical experience in monitoring system performance, checking disk usage, rotating log files, analyzing logs for errors, and setting up automated alerting. These skills are crucial for any Linux system administrator or DevOps role.

# Server Health Monitor

A lightweight Bash-based server monitoring tool that automatically checks system health metrics and logs alerts when thresholds are breached. Built as a Phase 1 capstone project in a self-taught Cloud/DevOps engineering roadmap.

---

## What It Monitors

| Check | Description |
|---|---|
| Disk Usage | Alerts when disk usage exceeds configured threshold |
| CPU Usage | Alerts when CPU usage exceeds configured threshold |
| Memory Usage | Alerts when RAM usage exceeds configured threshold |
| Service Status | Checks if a configured service (e.g. Nginx) is running |
| HTTP Response | Hits a URL and alerts if response is not HTTP 200 |
| DNS Resolution | Verifies the server can resolve external domain names |

---

## Skills Demonstrated

- Bash scripting — functions, variables, conditionals, error handling
- Linux system administration — services, disk, CPU, memory
- Networking — HTTP checks, DNS resolution, port monitoring
- Git/GitHub — proper commit history, secrets management with .gitignore
- Automation — cron scheduling

---
## Sample Output
```
2025-03-28 14:32:01 =============================================
2025-03-28 14:32:01 Health check started on kali
2025-03-28 14:32:01 =============================================
2025-03-28 14:32:01 Disk usage: 23%
2025-03-28 14:32:01 CPU usage is 4%
2025-03-28 14:32:01 Memory usage 61%
2025-03-28 14:32:01 Service nginx is running
2025-03-28 14:32:01 URL checked : HTTP 200
2025-03-28 14:32:01 DNS resolution checked - google.com resolved
2025-03-28 14:32:01 =============================================
2025-03-28 14:32:01 Health check completed
2025-03-28 14:32:01 =============================================
```

 ---
 
## Project Structure
```
server-monitor/
├── monitor.sh            # Main monitoring script
├── config.env            # Your local config (not tracked by Git)
├── config.env.example    # Safe config template
├── .gitignore            # Blocks secrets and logs from Git
├── README.md             # Project documentation
└── logs/
    ├── report.log        # Full timestamped health check history
    ├── alerts.log        # Only triggered alerts
    └── .gitkeep          # Keeps logs/ folder tracked by Git
```

---

## Setup

### 1. Clone the repository
```bash
git clone git@github.com:YOUR_USERNAME/server-monitor.git
cd server-monitor
```

### 2. Create your config file
```bash
cp config.env.example config.env
nano config.env
```

### 3. Set your thresholds and paths
```bash
# Thresholds (in percentage)
DISK_THRESHOLD=80
CPU_THRESHOLD=75
MEM_THRESHOLD=80

# Service to monitor
SERVICE_NAME=nginx

# Full paths to log files
REPORT_LOG=/full/path/to/server-monitor/logs/report.log
ALERT_LOG=/full/path/to/server-monitor/logs/alerts.log

# URL to check
CHECK_URL=http://localhost
```

### 4. Make the script executable
```bash
chmod +x monitor.sh
```

### 5. Run manually to verify
```bash
./monitor.sh
cat logs/report.log
```

---

## Automate with Cron

To run the monitor every 5 minutes automatically:
```bash
crontab -e
```

Add this line at the bottom:
```
*/5 * * * * /bin/bash /full/path/to/server-monitor/monitor.sh >> /full/path/to/server-monitor/logs/cron.log 2>&1
```

Verify it was added:
```bash
crontab -l
```

---

## How to Read the Logs

### Full health report
```bash
cat logs/report.log
```

Output looks like:
```
2025-03-28 14:32:01 =============================================
2025-03-28 14:32:01 Health check started on kali
2025-03-28 14:32:01 =============================================
2025-03-28 14:32:01 Disk usage: 23%
2025-03-28 14:32:01 CPU usage is 4%
2025-03-28 14:32:01 Memory usage 61%
2025-03-28 14:32:01 Service nginx is running
2025-03-28 14:32:01 URL checked : HTTP 200
2025-03-28 14:32:01 DNS resolution checked - google.com resolved
2025-03-28 14:32:01 =============================================
2025-03-28 14:32:01 Health check completed
2025-03-28 14:32:01 =============================================
```

### Alerts only
```bash
cat logs/alerts.log
```

---

## Testing Alerts

### Trigger a service alert
```bash
sudo systemctl stop nginx
./monitor.sh
cat logs/alerts.log
```

### Trigger a disk alert
Temporarily lower the threshold in `config.env`:
```bash
DISK_THRESHOLD=5
```
Run the script — a disk alert should fire. Set it back to `80` afterwards.

### Trigger a URL alert
```bash
sudo systemctl stop nginx
./monitor.sh
cat logs/alerts.log
```

---

## Requirements

- Linux (Debian/Ubuntu/Kali)
- Bash 4+
- curl
- dnsutils (`sudo apt install dnsutils -y`)
- Nginx (`sudo apt install nginx -y`)


---

## Author

**Mudassir**
CS Student — Sindh Madressatul Islam University, Karachi
Self-studying Cloud & DevOps Engineering
[GitHub](https://github.com/mudassir-secops) · [LinkedIn](https://linkedin.com/in/mudassir-48a691316/)

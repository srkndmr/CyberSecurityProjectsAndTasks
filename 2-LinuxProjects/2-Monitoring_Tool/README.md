

# System Monitoring Tool

This is a custom-built Bash-based system monitoring tool designed to track system metrics, log data, and send alerts or reports. It includes a user-friendly interface created using `dialog`.

---

## Features

- **Metric Collection**:
  - RAM Usage
  - Disk Usage
  - CPU Usage
  - System Uptime
- **Interactive Interface**:
  - View real-time metrics
  - Update thresholds dynamically
  - View logs and generate reports
- **Logging**:
  - Logs collected metrics into a CSV file.
- **Alerts**:
  - Sends email alerts when RAM, Disk, or CPU usage exceeds thresholds.
- **Weekly Reports**:
  - Generates a system usage report and emails it.
- **Automation**:
  - Configured to run hourly using `cron`.

---

## File Overview and Code

### `monitor.sh`
The main script for collecting metrics, logging them, and sending alerts if thresholds are breached.

```bash
#!/bin/bash

# Load configuration
source config.sh

# Get system metrics
TIMESTAMP=$(date +"%Y-%m-%d %H:%M:%S")
RAM_USAGE=$(free | awk '/Mem/{printf("%.0f", $3/$2*100)}')
DISK_USAGE=$(df / | awk '/\// {printf("%.0f", $3/$2*100)}')
CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1}')
UPTIME=$(uptime -p)

# Log the metrics
echo "$TIMESTAMP,$RAM_USAGE,$DISK_USAGE,$CPU_USAGE,$UPTIME" >> "$LOG_FILE"

# Interactive user interface using dialog
dialog --title "System Monitoring Tool" --menu "Choose an option:" 15 50 5 \
1 "View Current Metrics" \
2 "Update Thresholds" \
3 "View Log File" \
4 "Generate Weekly Report" 2>choice.txt

choice=$(<choice.txt)
case $choice in
1)
    dialog --msgbox "Metrics:\nRAM Usage: $RAM_USAGE%\nDisk Usage: $DISK_USAGE%\nCPU Usage: $CPU_USAGE%\nUptime: $UPTIME" 10 50
    ;;
2)
    dialog --inputbox "Enter new RAM threshold (%):" 10 50 2>ram.txt
    RAM_THRESHOLD=$(<ram.txt)
    dialog --inputbox "Enter new Disk threshold (%):" 10 50 2>disk.txt
    DISK_THRESHOLD=$(<disk.txt)
    dialog --inputbox "Enter new CPU threshold (%):" 10 50 2>cpu.txt
    CPU_THRESHOLD=$(<cpu.txt)
    echo "Updated thresholds: RAM=$RAM_THRESHOLD, Disk=$DISK_THRESHOLD, CPU=$CPU_THRESHOLD"
    ;;
3)
    dialog --textbox "$LOG_FILE" 20 60
    ;;
4)
    ./report.sh
    dialog --msgbox "Weekly report generated and sent!" 10 50
    ;;
esac

# Check thresholds and send alerts if necessary
if [ "$RAM_USAGE" -gt "$RAM_THRESHOLD" ]; then
    echo "Critical: RAM usage is at ${RAM_USAGE}%" | mail -s "RAM Alert" "$EMAIL"
fi

if [ "$DISK_USAGE" -gt "$DISK_THRESHOLD" ]; then
    echo "Critical: Disk usage is at ${DISK_USAGE}%" | mail -s "Disk Alert" "$EMAIL"
fi

if [ "$CPU_USAGE" -gt "$CPU_THRESHOLD" ]; then
    echo "Critical: CPU usage is at ${CPU_USAGE}%" | mail -s "CPU Alert" "$EMAIL"
fi
```

---

### `config.sh`
The configuration file that contains the thresholds, email settings, and file paths.

```bash
#!/bin/bash
# Configuration file for monitoring script

# Email address for notification
EMAIL="your-email@example.com"

# Thresholds for alerts
RAM_THRESHOLD=80  # Percentage
DISK_THRESHOLD=90 # Percentage
CPU_THRESHOLD=85  # Percentage

# Log file
LOG_FILE="/home/user/monitoring_tool/logfile.log"

# Report file
REPORT_FILE="/home/user/monitoring_tool/report.txt"
```

---

### `cron_setup.sh`
Sets up a cron job to run the `monitor.sh` script every hour.

```bash
#!/bin/bash
# Set up cron jobs

# Remove existing cron jobs for this script (optional)
crontab -l | grep -v 'monitor.sh' | crontab -

# Add new cron job to run monitor.sh every hour
(crontab -l 2>/dev/null; echo "0 * * * * /home/user/monitoring_tool/monitor.sh") | crontab -
```

---

### `report.sh`
Generates a weekly report from logged data and emails it.

```bash
#!/bin/bash

# Load configuration
source config.sh

# Generate report
echo "Weekly System Report" > "$REPORT_FILE"
cat "$LOG_FILE" >> "$REPORT_FILE"

# Send report via email
if mail -s "Weekly System Report" "$EMAIL" < "$REPORT_FILE"; then
    echo "Email sent successfully." >> script.log
else
    echo "Failed to send email." >> script.log
fi
```

---

## Prerequisites

1. **Dialog**:
   ```bash
   sudo apt install dialog
   ```
2. **Mailutils**:
   ```bash
   sudo apt install mailutils
   ```
3. **Cron**:
   Pre-installed on most Debian-based systems.

---

## Installation and Setup

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/system-monitoring-tool.git
   cd system-monitoring-tool
   ```

2. **Set Permissions**:
   ```bash
   chmod +x monitor.sh config.sh cron_setup.sh report.sh
   ```

3. **Configure Settings**:
   Edit the `config.sh` file to set:
   - Your email address.
   - Threshold values for RAM, Disk, and CPU usage.

4. **Set Up Cron**:
   Run the following command to set up the cron job:
   ```bash
   ./cron_setup.sh
   ```

5. **Run the Tool**:
   Execute the monitoring tool interactively:
   ```bash
   ./monitor.sh
   ```

---


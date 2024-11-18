
# System Monitoring Tool

This project is a custom-built system monitoring script designed to collect system metrics, log data, and send alerts or reports via email. It is implemented in Bash, allowing you to monitor the state of your system and get notified when critical thresholds are breached.

---

## Features

- **Metric Collection**: Collects metrics including:
  - RAM Usage
  - Disk Usage
  - CPU Usage
  - System Uptime
- **Logging**: Logs collected metrics into a CSV file for analysis.
- **Alerts**: Sends email alerts when RAM, Disk, or CPU usage exceeds predefined thresholds.
- **Weekly Reports**: Generates a system usage report and emails it to the configured address.
- **Automation**: Scheduled to run hourly using `cron`.
- **Interactive User Interface**: Includes a dialog-based menu for seamless interaction with the monitoring tool.

---

## File Descriptions

### `monitor.sh`

This is the main script for collecting and logging system metrics. It checks against thresholds and sends email alerts if necessary.

#!/bin/bash

# Load configuration
source config.sh

# Function to display metrics using dialog
display_metrics() {
    TIMESTAMP=$(date +"%Y-%m-%d %H:%M:%S")
    RAM_USAGE=$(free | awk '/Mem/{printf("%.0f", $3/$2*100)}')
    DISK_USAGE=$(df / | awk '/\// {printf("%.0f", $3/$2*100)}')
    CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1}')
    UPTIME=$(uptime -p)

    # Display metrics
    dialog --msgbox "Timestamp: $TIMESTAMP\nRAM Usage: ${RAM_USAGE}%\nDisk Usage: ${DISK_USAGE}%\nCPU Usage: ${CPU_USAGE}%\nUptime: $UPTIME" 10 50
}

# Function to update thresholds
update_thresholds() {
    RAM_THRESHOLD=$(dialog --inputbox "Enter new RAM threshold (current: $RAM_THRESHOLD):" 10 50 "$RAM_THRESHOLD" 3>&1 1>&2 2>&3)
    DISK_THRESHOLD=$(dialog --inputbox "Enter new Disk threshold (current: $DISK_THRESHOLD):" 10 50 "$DISK_THRESHOLD" 3>&1 1>&2 2>&3)
    CPU_THRESHOLD=$(dialog --inputbox "Enter new CPU threshold (current: $CPU_THRESHOLD):" 10 50 "$CPU_THRESHOLD" 3>&1 1>&2 2>&3)
}

# Function to view log file
view_log() {
    dialog --textbox "$LOG_FILE" 20 60
}

# Function to generate weekly report
generate_report() {
    ./report.sh
    dialog --msgbox "Weekly report generated and emailed to $EMAIL." 10 50
}

# Main menu loop
while true; do
    CHOICE=$(dialog --menu "System Monitoring Tool" 15 50 5 \
        1 "View Current Metrics" \
        2 "Update Thresholds" \
        3 "View Log File" \
        4 "Generate Weekly Report" \
        5 "Exit" 3>&1 1>&2 2>&3)

    case $CHOICE in
        1) display_metrics ;;
        2) update_thresholds ;;
        3) view_log ;;
        4) generate_report ;;
        5) clear; exit 0 ;;
    esac
done


### `config.sh`

Contains the configuration details, including:
- Email address for notifications
- Threshold values for RAM, Disk, and CPU usage
- File paths for logs and reports

  #!/bin/bash
# Configuration file for monitoring script

# Email address for notification
EMAIL="dmrsrknn@gmail.com"

# Thresholds for alerts
RAM_THRESHOLD=80  # Percentage
DISK_THRESHOLD=90 # Percentage
CPU_THRESHOLD=85  # Percentage

# Log file
LOG_FILE="/home/srkn/projects/monitoring_tool/logfile.log"

# Report file
REPORT_FILE="/home/srkn/projects/monitoring_tool/report.txt"


### `cron_setup.sh`

Sets up a cron job to execute the `monitor.sh` script every hour automatically.

#!/bin/bash
# Set up cron jobs

# Remove existing cron jobs for this script (optional)
crontab -l | grep -v 'monitor.sh' | crontab -

# Add new cron job to run monitor.sh every hour
(crontab -l 2>/dev/null; echo "0 * * * * /home/srkn/projects/monitoring_tool/monitor.sh") | crontab -


### `report.sh`

Generates a weekly system report from logged data and emails it using `smstp`.

#!/bin/bash

# Load configuration
source config.sh

# Generate report
echo "Weekly System Report" > "$REPORT_FILE"
cat "$LOG_FILE" >> "$REPORT_FILE"

# Send report via email using smstp
if smstp --account=default "$EMAIL" < "$REPORT_FILE"; then
    echo "Email sent successfully." >> script.log
else
    echo "Failed to send email." >> script.log
fi


## User Interface

The monitoring tool features an interactive, menu-driven user interface built using `dialog`. This interface provides a graphical terminal experience for interacting with the script.
sudo apt install dialog
sudo apt install mailutils

### Features of the Dialog-Based UI:
1. **Main Menu**: A menu with options to:
   - View Current Metrics
   - Update Thresholds
   - View Log File
   - Generate Weekly Report
   - Exit

2. **Menu Navigation**:
   - Use arrow keys to navigate through the options.
   - Select an option by pressing Enter.

3. **Dialog Elements**:
   - **Message Box**: Displays metrics and alerts.
   - **Input Box**: Allows updating of thresholds interactively.
   - **Text Box**: Opens and scrolls through the log file.
   - **Notification Box**: Confirms the generation and sending of reports.

### Example UI Interaction

On running the script, you will see a menu similar to this:


- **Option 1**: Displays real-time metrics like RAM, CPU, and Disk usage.
- **Option 2**: Prompts for new threshold values.
- **Option 3**: Opens the log file for review.
- **Option 4**: Generates and emails a weekly report.

---

## Prerequisites

- **Bash**: Required for executing the scripts.
- **Dialog**: Install `dialog` using your package manager:
  ```bash
  sudo apt install dialog  # For Debian/Ubuntu
  sudo dnf install dialog  # For Fedora

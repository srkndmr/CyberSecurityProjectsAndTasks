
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

### `config.sh`

Contains the configuration details, including:
- Email address for notifications
- Threshold values for RAM, Disk, and CPU usage
- File paths for logs and reports

### `cron_setup.sh`

Sets up a cron job to execute the `monitor.sh` script every hour automatically.

### `report.sh`

Generates a weekly system report from logged data and emails it using `smstp`.

---

## User Interface

The monitoring tool features an interactive, menu-driven user interface built using `dialog`. This interface provides a graphical terminal experience for interacting with the script.

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

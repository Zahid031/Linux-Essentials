# Linux Logs & Monitoring for DevOps

Effective logging and monitoring are crucial for maintaining system health, troubleshooting issues, and ensuring performance. This guide covers the essential tools for log inspection and real-time monitoring on Linux.

### Table of Contents
1.  [The `systemd` Journal](#1-the-systemd-journal)
2.  [Traditional Log Files](#2-traditional-log-files)
3.  [Kernel & Boot Messages](#3-kernel--boot-messages)
4.  [Real-time Process Monitoring](#4-real-time-process-monitoring)
5.  [The Next Step: Centralized Monitoring](#5-the-next-step-centralized-monitoring)

---

## 1. The `systemd` Journal

On modern Linux distributions, `journalctl` is the primary interface for querying the structured logs collected by `systemd-journald`.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `journalctl` | Query the `systemd` journal. | `journalctl`: Show all logs, starting with the oldest. <br> `journalctl -b`: Show all messages from the current boot. <br> `journalctl -b -1`: Show messages from the *previous* boot. |
| `journalctl -f` | **F**ollow the journal, showing new messages in real-time. | `journalctl -f` |
| `journalctl -u` | Filter logs for a specific `systemd` **u**nit (service). | `journalctl -u nginx.service`: Show all logs from the `nginx` service. <br> `journalctl -u sshd -f`: Follow the logs for the SSH daemon. |
| `journalctl -p` | Filter logs by **p**riority (level). | `journalctl -p err`: Show all logs with a priority of "error" or higher. <br> `journalctl -p 3 -b`: Show error-level messages from the current boot. |
| `journalctl --since` | Show logs that have occurred since a specific time. | `journalctl --since "1 hour ago"` <br> `journalctl --since "2025-07-16 10:00:00"` |

---

## 2. Traditional Log Files

Even on `systemd` systems, many applications still write to plain-text log files in `/var/log`.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `tail` | View the end of a file. | `tail /var/log/syslog`: Show the last 10 lines. <br> `tail -n 100 /var/log/nginx/access.log`: Show the last 100 lines. |
| `tail -f` | **F**ollow a file, showing new lines as they are added. | `tail -f /var/log/auth.log`: Watch for new authentication events in real-time. |
| `less` | View a file interactively, allowing you to scroll and search. | `less +F /var/log/nginx/error.log`: Open the file and start in "follow" mode (like `tail -f`). Press `Ctrl+C` to stop following and search, then `Shift+F` to resume. |
| `grep` | Search for patterns within log files. | `grep "Failed password" /var/log/auth.log`: Find all failed login attempts. |

---

## 3. Kernel & Boot Messages

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `dmesg` | Print or control the kernel ring buffer. | `dmesg`: Show all kernel messages since boot. <br> `dmesg -T`: Show human-readable timestamps. <br> `dmesg | grep -i "error"`: Filter for kernel-level error messages. |

---

## 4. Real-time Process Monitoring

These tools provide a live dashboard of your system's resource usage.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `top` | The classic real-time process monitor. | `top`: Press `P` to sort by CPU, `M` for Memory, `k` to kill, `q` to quit. |
| `htop` | An enhanced, user-friendly process monitor. | `htop`: Offers color, mouse support, and easier sorting/filtering. (May require `sudo apt install htop`). |
| `glances` | A powerful, cross-platform monitoring tool with more detail than `top` or `htop`. | `glances`: Shows CPU, memory, disk I/O, network stats, and more in one view. (May require `sudo apt install glances`). |
| `vmstat` | **V**irtual **M**emory **Stat**istics. Reports information about processes, memory, paging, block IO, traps, and cpu activity. | `vmstat 2`: Run every 2 seconds. Good for spotting I/O bottlenecks (`bi`, `bo`) or high context switching (`cs`). |

---

## 5. The Next Step: Centralized Monitoring

For production environments, command-line tools are used for immediate troubleshooting, but a centralized monitoring solution is essential for long-term health and alerting.

*   **Prometheus:** An open-source systems monitoring and alerting toolkit that collects metrics from configured targets at given intervals.
*   **Grafana:** An open-source platform for monitoring and observability that allows you to query, visualize, alert on, and explore your metrics, logs, and traces.
*   **Loki:** A log aggregation system designed to store and query logs from all your applications and infrastructure.

These tools work together to provide a comprehensive overview of your entire infrastructure's health.

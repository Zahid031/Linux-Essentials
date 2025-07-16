# Linux Process & Service Management: The DevOps Cheat Sheet

This guide is a comprehensive, "cheat-sheet style" reference that DevOps engineers use for day-to-day process and service management on modern Linux distributions (especially those using `systemd`).

It's designed to be interactive and informative, providing not just the commands, but also the context and details you need to use them effectively.

### Table of Contents
1.  [Inspect & Monitor Running Processes](#1-inspect--monitor-running-processes)
2.  [Find Processes by Name or Criteria](#2-find-processes-by-name-or-criteria)
3.  [Control Processes (Signals & Priorities)](#3-control-processes-signals--priorities)
4.  [Modern `systemd` Service Management](#4-modern-systemd-service-management)
5.  [Log Inspection with `journalctl`](#5-log-inspection-with-journalctl)
6.  [Network & File Inspection](#6-network--file-inspection)
7.  [Legacy SysVinit Commands](#7-legacy-sysvinit-commands)
8.  [Automation & Scheduling](#8-automation--scheduling)
9.  [Quick Troubleshooting Workflow](#9-quick-troubleshooting-workflow)

---

## 1. Inspect & Monitor Running Processes

These tools give you a snapshot or a live view of what's happening on your server.

| Goal | Command(s) | In-Depth Explanation | Example |
| :--- | :--- | :--- | :--- |
| **Snapshot of All Processes** | `ps aux` | `a` = show processes for all users, `u` = display user-oriented format, `x` = show processes not attached to a terminal. This is the classic "show me everything" command. | `ps aux \| grep nginx` |
| **Tree View (Parent/Child)** | `ps auxf` | The `f` flag adds ASCII art tree view, showing which processes spawned others. Excellent for tracing process lineage. | `ps auxf \| less` |
| **Custom Columns, Sorted** | `ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu` | `eo` lets you define exact columns. Here, we sort by memory usage (`-%mem`). Great for finding resource hogs. | `ps -eo pid,cmd,%mem --sort=-%mem \| head -n 10` |
| **Real-time Interactive View** | `top` | A real-time dashboard of system processes. Press `P` to sort by CPU, `M` for Memory, `k` to kill a process, and `q` to quit. | `top` |
| **Enhanced `top`** | `htop` | A user-friendly, colorful, and interactive version of `top`. Allows scrolling, mouse use, and easier killing/renicing. (May require `sudo apt install htop`). | `htop` |
| **Per-Process I/O Stats** | `pidstat 2` | Part of the `sysstat` package. Shows real-time CPU, memory, and I/O stats per process, updating every 2 seconds. | `pidstat -p $(pgrep -d, java) 2` |

> **Pro-Tip:** Pipe `ps` output to `less` (e.g., `ps aux | less`) to scroll through a long list of processes without flooding your terminal.

---

## 2. Find Processes by Name or Criteria

Instead of `ps | grep`, these are more direct and safer tools.

| Goal | Command(s) | In-Depth Explanation | Example |
| :--- | :--- | :--- | :--- |
| **Find Process ID by Name** | `pgrep <name>` | Searches for processes by name and returns only the PIDs. The `-l` flag will also list the process name. | `pgrep nginx` |
| **Find Process by User** | `pgrep -u <user>` | Find all processes running as a specific user. | `pgrep -u www-data` |

---

## 3. Control Processes (Signals & Priorities)

Sending signals tells processes what to do.

| Goal | Command(s) | In-Depth Explanation | Example |
| :--- | :--- | :--- | :--- |
| **Graceful Termination** | `kill <PID>` | Sends `SIGTERM` (signal 15), asking the process to shut down cleanly. This is the polite and safe way to stop something. | `kill 1234` |
| **Force Kill** | `kill -9 <PID>` | Sends `SIGKILL` (signal 9), which terminates the process immediately without cleanup. Use this as a last resort for stuck processes. | `kill -9 1234` |
| **Kill by Name** | `pkill <name>` | A convenient combination of `pgrep` and `kill`. It finds and kills processes by name. Use with caution! | `pkill -f 'stale-worker.py'` |
| **Send Reload Signal** | `kill -HUP <PID>` | Sends `SIGHUP` (Hang Up), often used to tell a service to reload its configuration file without restarting. | `kill -HUP $(cat /var/run/nginx.pid)` |
| **Start with Lower Priority** | `nice -n 10 <cmd>` | Runs a command with a lower CPU priority (`nice` values range from -20 (highest) to 19 (lowest)). | `nice -n 15 ./backup-script.sh` |
| **Change Priority of Running Process** | `renice <val> -p <PID>` | Changes the `nice` value of an already running process. | `renice 5 -p 1234` |

---

## 4. Modern `systemd` Service Management

`systemctl` is the primary tool for controlling the `systemd` init system. **Always use `sudo` for commands that change state.**

| Goal | Command(s) | In-Depth Explanation | Example |
| :--- | :--- | :--- | :--- |
| **Start a Service** | `systemctl start <svc>` | Starts a service immediately. The service will stop on reboot unless enabled. | `sudo systemctl start docker` |
| **Stop a Service** | `systemctl stop <svc>` | Stops a service immediately. | `sudo systemctl stop apache2` |
| **Restart (Stop then Start)** | `systemctl restart <svc>` | The most common way to "bounce" a service. | `sudo systemctl restart nginx` |
| **Reload Config Gracefully** | `systemctl reload <svc>` | Reloads the service's configuration without dropping connections. Much better than a restart if the service supports it. | `sudo systemctl reload nginx` |
| **Enable at Boot** | `systemctl enable <svc>` | Ensures the service starts automatically when the system boots. | `sudo systemctl enable jenkins` |
| **Disable from Boot** | `systemctl disable <svc>` | Prevents the service from starting at boot. | `sudo systemctl disable docker` |
| **Check Status & Logs** | `systemctl status <svc>` | Shows if the service is running, its PID, memory usage, and the last few log lines. Your first step in debugging! | `systemctl status kubelet` |
| **List Running Services** | `systemctl list-units --type=service --state=running` | Gives a quick overview of all currently active services. | `systemctl list-units --type=service` |
| **Prevent a Service from Ever Starting** | `systemctl mask <svc>` | Links the service to `/dev/null`. It cannot be started manually or by another service until `unmask`ed. | `sudo systemctl mask chrony` |

---

## 5. Log Inspection with `journalctl`

`journalctl` is the powerful tool for querying the `systemd` journal (logs).

| Goal | Command(s) | In-Depth Explanation | Example |
| :--- | :--- | :--- | :--- |
| **All Logs Since Boot** | `journalctl -b` | `-b` shows all messages from the current boot. Use `-b -1` for the previous boot, and so on. | `journalctl -b \| less` |
| **Follow Live Logs** | `journalctl -f` | "Follows" the journal, showing new messages in real-time. The equivalent of `tail -f` for modern Linux. | `journalctl -f` |
| **Service-Specific Logs** | `journalctl -u <svc>` | `-u` filters logs for a specific `systemd` unit (service). This is essential for debugging a single application. | `journalctl -u nginx.service -f` |
| **Filter by Time** | `journalctl --since "YYYY-MM-DD HH:MM:SS"` | Show logs that have occurred since a specific time. You can also use `--until`. | `journalctl --since "10 min ago"` |
| **Filter by Priority** | `journalctl -p <level>` | Filter logs by severity, from `emerg` (0) to `debug` (7). `err` (3) is very common for finding problems. | `journalctl -p err -b` |

---

## 6. Network & File Inspection

Troubleshooting often involves checking what ports are open or what files a process is using.

| Goal | Command(s) | In-Depth Explanation | Example |
| :--- | :--- | :--- | :--- |
| **List Listening Sockets** | `ss -tulnp` | `ss` is the modern replacement for `netstat`. Flags: `t`=TCP, `u`=UDP, `l`=listening, `n`=numeric ports, `p`=show process. | `sudo ss -tulnp \| grep 8080` |
| **List Open Files by Process** | `lsof -p <PID>` | `lsof` = List Open Files. See every file and network connection a process has open. Incredibly powerful for debugging. | `lsof -p 1234` |
| **Who is Using a Port?** | `lsof -i :<port>` | The inverse of the above: find which process is listening on a specific network port. | `sudo lsof -i :443` |

---

## 7. Legacy SysVinit Commands

You might find these on older systems (like CentOS 6) or on systems that still have some legacy scripts.

| Goal | Command(s) | Example |
| :--- | :--- | :--- |
| **Start/Stop/Restart** | `service <svc> start/stop/restart` | `sudo service httpd restart` |
| **Check Status** | `service <svc> status` | `service mysql status` |

---

## 8. Automation & Scheduling

| Hook | Command(s) | DevOps Use-Case |
| :--- | :--- | :--- |
| **Cron (Classic)** | `crontab -e` | Schedule a recurring job, like a nightly backup script. Simple and reliable. |
| **systemd Timers** | `*.timer` and `*.service` files | A modern replacement for cron. Offers better logging, resource control, and dependency management. More complex but more powerful. |
| **One-off Job** | `at 23:00` | Schedule a command to run just once at a future time. Press `Ctrl-D` to finish. |

---

## 9. Quick Troubleshooting Workflow

Here is a logical sequence of commands to diagnose a failing application.

1.  **Check the service status.** This tells you if `systemd` *thinks* the service is running and shows the latest logs.
    ```bash
    sudo systemctl status my-app.service
    ```
2.  **If it's not running, try to start it.**
    ```bash
    sudo systemctl start my-app.service
    ```
3.  **If it fails to start, check the live logs** while you attempt to start it in another terminal. This often reveals the root cause immediately.
    ```bash
    sudo journalctl -u my-app.service -f
    ```
4.  **If it *is* running but not working, check for resource issues** or if it's listening on the correct port.
    ```bash
    htop
    sudo ss -tulnp | grep my-app
    ```
5.  **As a last resort, try a full restart.**
    ```bash
    sudo systemctl restart my-app.service
    ```

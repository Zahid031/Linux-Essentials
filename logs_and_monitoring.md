# Linux Logs & Monitoring for DevOps

Effective logging and monitoring are crucial for maintaining system health, troubleshooting issues, and ensuring performance. This guide covers the essential tools for log inspection and real-time monitoring on Linux.

### Table of Contents
1.  [The `systemd` Journal](#1-the-systemd-journal)
2.  [Inspecting Log Files](#2-inspecting-log-files)
3.  [Advanced Log Analysis with Pipes](#3-advanced-log-analysis-with-pipes)
4.  [Kernel & Boot Messages](#4-kernel--boot-messages)
5.  [Real-time Process Monitoring](#5-real-time-process-monitoring)

---

## 1. The `systemd` Journal

On modern Linux distributions, `journalctl` is the primary interface for querying the structured logs collected by `systemd-journald`.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `journalctl` | Query the `systemd` journal. | `journalctl`: Show all logs, starting with the oldest. <br> `journalctl -b`: Show all messages from the current boot. |
| `journalctl -f` | **F**ollow the journal, showing new messages in real-time. | `journalctl -f` |
| `journalctl -u` | Filter logs for a specific `systemd` **u**nit (service). | `journalctl -u nginx.service`: Show all logs from the `nginx` service. |
| `journalctl -p` | Filter logs by **p**riority (level). | `journalctl -p err`: Show all logs with a priority of "error" or higher. |
| `journalctl --since` | Show logs that have occurred since a specific time. | `journalctl --since "1 hour ago"` |

---

## 2. Inspecting Log Files

Many applications still write to plain-text log files, typically in `/var/log`.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `tail` | View the end of a file. | `tail -n 100 /var/log/nginx/access.log`: Show the last 100 lines. |
| `tail -f` | **F**ollow a file for live updates. | `tail -f /var/log/auth.log` |
| `head` | View the beginning of a file. | `head -n 50 /var/log/dmesg` |
| `less` | View a file interactively (scroll and search). | `less /var/log/nginx/error.log` |
| `grep` | Search for patterns within files. | `grep "Failed password" /var/log/auth.log` |

---

## 3. Advanced Log Analysis with Pipes

The true power of the command line comes from the pipe (`|`), which sends the output of one command to be the input of the next. This allows you to build powerful, one-line analysis scripts.

### Deconstruction of an Example

Let's find the **top 10 IP addresses** hitting an Nginx server.

**Command:** `cat /var/log/nginx/access.log | awk '{print $1}' | sort | uniq -c | sort -nr | head -n 10`

1.  `cat /var/log/nginx/access.log`
    *   **Action:** Reads the entire log file and prints its content to standard output.
    *   **Output:** Every single line of the log file.

2.  `... | awk '{print $1}'`
    *   **Action:** `awk` processes the text line by line. `{print $1}` tells it to print only the first column (the IP address in a standard Nginx log).
    *   **Output:** A long list of just the IP addresses, one per line.

3.  `... | sort`
    *   **Action:** Takes the list of IPs and sorts them alphabetically/numerically. This is crucial for `uniq` to work correctly.
    *   **Output:** The same list of IPs, but now all identical IPs are adjacent to each other.

4.  `... | uniq -c`
    *   **Action:** `uniq` removes duplicate lines. The `-c` flag tells it to also **c**ount how many times each line occurred.
    *   **Output:** A list of unique IPs, prefixed with their hit count. (e.g., `5 192.168.1.10`)

5.  `... | sort -nr`
    *   **Action:** Sorts the list again. `-n` specifies a **n**umeric sort (not alphabetical), and `-r` reverses it to be in descending order.
    *   **Output:** The list of unique IPs, now sorted from most frequent to least frequent.

6.  `... | head -n 10`
    *   **Action:** Shows only the top 10 lines of the sorted list.
    *   **Final Output:** The top 10 most frequent IP addresses.

### Common Log Analysis Recipes

| Goal | Command Chain | Explanation |
| :--- | :--- | :--- |
| **Count unique errors** | `grep -i "error" app.log \| sort \| uniq -c` | 1. `grep` finds all lines with "error". <br> 2. `sort` groups identical error messages together. <br> 3. `uniq -c` counts each unique error. |
| **Find top 5 requested pages** | `awk -F'"' '{print $2}' access.log \| awk '{print $2}' \| sort \| uniq -c \| sort -nr \| head -n 5` | 1. `awk -F'"'` splits the line by quotes to isolate the request string (e.g., `GET /page HTTP/1.1`). <br> 2. The second `awk` extracts just the URL. <br> 3. The rest sorts, counts, and finds the top 5. |
| **Summarize HTTP status codes** | `awk '{print $9}' access.log \| sort \| uniq -c \| sort -nr` | 1. `awk` prints the 9th column (status code). <br> 2. The rest of the chain counts each occurrence of the codes (200, 404, 500, etc.). |
| **Find failed logins for a user** | `grep "Failed password for" /var/log/auth.log \| grep "user_name" \| wc -l` | 1. First `grep` finds all failed password attempts. <br> 2. Second `grep` filters those lines for a specific username. <br> 3. `wc -l` counts the resulting lines. |

---

## 4. Kernel & Boot Messages

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `dmesg` | Print or control the kernel ring buffer. | `dmesg`: Show all kernel messages since boot. <br> `dmesg -T`: Show human-readable timestamps. <br> `dmesg | grep -i "error"`: Filter for kernel-level error messages. |

---

## 5. Real-time Process Monitoring

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `top` | The classic real-time process monitor. | `top`: Press `P` to sort by CPU, `M` for Memory, `k` to kill, `q` to quit. |
| `htop` | An enhanced, user-friendly process monitor. | `htop`: Offers color, mouse support, and easier sorting/filtering. (May require `sudo apt install htop`). |
| `vmstat` | **V**irtual **M**emory **Stat**istics. | `vmstat 2`: Run every 2 seconds. Good for spotting I/O bottlenecks (`bi`, `bo`) or high context switching (`cs`). |

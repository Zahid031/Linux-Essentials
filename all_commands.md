# Comprehensive Guide to Linux Commands

This document provides a comprehensive list of common Linux commands, categorized for easy reference. Each command includes a detailed description of its function, common flags, and practical examples to help you understand Linux effectively.

---

## 1. Basic Commands

These are the fundamental commands for navigating and interacting with the Linux shell.

| Command | Description | Examples |
|---|---|---|
| `ls` | Lists the contents of a directory. By default, it lists files and directories in the current location. | `ls` - List files in the current directory.<br>`ls -l` - List in long format, showing permissions, owner, size, and modification date.<br>`ls -a` - List all files, including hidden files (those starting with `.`).<br>`ls -lh` - List in long, human-readable format (e.g., 4.0K, 1.2M).<br>`ls -R` - List files in subdirectories recursively. |
| `cd` | Changes the current working directory. | `cd /var/log` - Change to an absolute path.<br>`cd ..` - Move up one directory level.<br>`cd` or `cd ~` - Go to your home directory.<br>`cd -` - Go to the previous directory you were in. |
| `pwd` | Prints the full path of the current working directory. | `pwd` |
| `cp` | Copies files or directories from a source to a destination. | `cp source.txt destination.txt` - Copy a file.<br>`cp -r source_dir/ destination_dir/` - Recursively copy a directory and its contents.<br>`cp -v source.txt dest_dir/` - Verbose mode, shows progress. |
| `mv` | Moves or renames files or directories. | `mv old_name.txt new_name.txt` - Rename a file.<br>`mv file.txt /tmp/` - Move a file to a different directory. |
| `rm` | Removes (deletes) files or directories. **Use with caution.** | `rm file.txt` - Delete a file.<br>`rm -r directory/` - Recursively delete a directory and its contents.<br>`rm -f file.txt` - Force deletion without prompting.<br>`rm -rf directory/` - Forcefully and recursively delete a directory. **Extremely dangerous.** |
| `mkdir` | Creates a new directory. | `mkdir new_folder`<br>`mkdir -p parent/child/grandchild` - Create parent directories as needed. |
| `rmdir` | Removes an *empty* directory. | `rmdir empty_folder` |
| `touch` | Creates an empty file or updates the access and modification timestamps of an existing file. | `touch new_file.txt` |
| `clear` | Clears the terminal screen. | `clear` |

---

## 2. User Management

Commands for managing user accounts and groups. Most of these require `sudo` privileges.

| Command | Description | Example (sudo for elevated privileges) |
|---|---|---|
| `useradd` | Adds a new user to the system. | `sudo useradd -m -s /bin/bash newuser` - Create user with a home directory and default shell. |
| `groupadd` | Creates a new user group. | `sudo groupadd developers` |
| `adduser` | An interactive, user-friendly script to add a new user. | `sudo adduser newuser` |
| `passwd` | Changes a user's password. | `passwd` - Change your own password.<br>`sudo passwd someuser` - Change another user's password. |
| `usermod` | Modifies a user account's properties. | `sudo usermod -aG sudo newuser` - Add a user to the `sudo` group.<br>`sudo usermod -s /bin/zsh newuser` - Change a user's default shell. |
| `deluser` | Deletes a user from the system. | `sudo deluser someuser` |
| `userdel` | Deletes a user, with an option to remove their home directory. | `sudo userdel -r someuser` - Delete user and their home directory. |
| `groupdel` | Deletes an existing user group. | `sudo groupdel developers` |
| `groups` | Shows the groups a user belongs to. | `groups someuser` |
| `whoami` | Displays the current logged-in user's username. | `whoami` |
| `chage` | Changes user account aging information (e.g., password expiration). | `sudo chage -l someuser` - List aging information.<br>`sudo chage -M 90 someuser` - Set password to expire in 90 days. |
| `who` | Shows who is currently logged on to the system. | `who` |
| `w` | Shows who is logged in and what they are doing. | `w` |
| `id` | Displays user ID (UID), group ID (GID), and group memberships. | `id someuser` |

---

## 3. File and Directory Management

Commands for finding and managing files and directories.

| Command | Description | Example |
|---|---|---|
| `find` | Searches for files in a directory hierarchy based on various criteria. | `find /home -name "*.txt"` - Find all `.txt` files in `/home`.<br>`find . -type d` - Find all directories in the current location.<br>`find / -size +1G` - Find files larger than 1GB.<br>`find . -mtime -7` - Find files modified in the last 7 days. |
| `locate` | Finds files by name using a pre-built database (`updatedb`). Much faster than `find`. | `locate my_document.pdf` |
| `tree` | Displays directories and their contents in a tree-like format. | `tree`<br>`tree -L 2` - Show only 2 levels deep. |
| `stat` | Displays detailed information about a file or filesystem, including timestamps. | `stat file.txt` |
| `basename` | Extracts the filename from a path. | `basename /etc/nginx/nginx.conf` -> `nginx.conf` |
| `dirname` | Extracts the directory path from a file path. | `dirname /etc/nginx/nginx.conf` -> `/etc/nginx` |
| `rsync` | Synchronizes files and directories between two locations (local or remote). It's efficient, transferring only the differences. | `rsync -avz source/ destination/` - Sync locally.<br>`rsync -avz -e ssh user@host:/remote/path/ /local/path/` - Sync from a remote server. |

---

## 4. File Permission & Ownership

Commands to control access to files and directories.

| Command | Description | Example |
|---|---|---|
| `chmod` | Changes file permissions. Can use octal (numeric) or symbolic notation. | `chmod 755 script.sh` - Sets `rwxr-xr-x`.<br>`chmod u+x script.sh` - Adds execute permission for the **u**ser.<br>`chmod g-w file.txt` - Removes write permission for the **g**roup.<br>`chmod -R 644 /var/www/html` - **R**ecursively set permissions for a directory. |
| `chown` | Changes the owner and group of a file or directory. | `sudo chown user:group file.txt`<br>`sudo chown -R user:group /opt/app` - **R**ecursively change ownership. |
| `chgrp` | Changes the group ownership of a file or directory. | `sudo chgrp developers file.txt` |
| `umask` | Sets the default permissions for newly created files and directories. | `umask 022` - (Default) Files are `644`, directories are `755`.<br>`umask 077` - (Secure) Files are `600`, directories are `700`. |

---

## 5. Process Management

Commands for managing running applications and processes.

| Command | Description | Example |
|---|---|---|
| `ps` | Displays a snapshot of currently running processes. | `ps aux` - Show all processes from all users.<br>`ps auxf` - Show processes in a parent-child tree view.<br>`ps -eo pid,ppid,%mem,%cpu,cmd --sort=-%mem` - Custom output sorted by memory usage. |
| `systemctl` | The primary tool for controlling the `systemd` system and service manager. | `sudo systemctl start nginx`<br>`sudo systemctl stop nginx`<br>`sudo systemctl restart nginx`<br>`sudo systemctl status nginx`<br>`sudo systemctl enable nginx` - Start on boot.<br>`sudo systemctl disable nginx` - Don't start on boot. |
| `top` | Shows a real-time, dynamic view of running processes. Interactive. | `top` - Press `P` to sort by CPU, `M` by Memory, `k` to kill, `q` to quit. |
| `kill` | Terminates a process by its Process ID (PID). | `kill 1234` - (SIGTERM) Graceful shutdown.<br>`kill -9 1234` - (SIGKILL) Forceful shutdown. |
| `killall` | Terminates processes by their name. | `killall -9 firefox` |
| `htop` | An interactive, user-friendly process viewer (often needs to be installed). | `htop` |
| `jobs` | Lists background jobs running in the current shell. | `jobs` |
| `bg` | Resumes a stopped job and runs it in the background. | `bg %1` |
| `fg` | Brings a background job to the foreground. | `fg %1` |
| `shutdown` | Shuts down, reboots, or halts the system. | `sudo shutdown -h now` - Halt immediately.<br>`sudo shutdown -r +10` - Reboot in 10 minutes. |
| `reboot` | A shortcut to reboot the system. | `sudo reboot` |
| `poweroff` | A shortcut to power off the system. | `sudo poweroff` |

---

## 6. Package Management

Commands for installing, updating, and removing software packages. The command depends on your Linux distribution.

| Distro Family | Command | Example |
|---|---|---|
| Debian/Ubuntu | `apt` / `apt-get` | `sudo apt update`<br>`sudo apt upgrade`<br>`sudo apt install htop`<br>`sudo apt remove htop`<br>`apt search nginx` |
| Red Hat/CentOS/Fedora | `yum` / `dnf` | `sudo dnf check-update`<br>`sudo dnf install htop`<br>`sudo dnf remove htop`<br>`dnf search nginx` |

---

## 7. Network Commands

Commands for configuring and troubleshooting network connections.

| Command | Description | Example |
|---|---|---|
| `ping` | Tests connectivity to a network host by sending ICMP packets. | `ping google.com`<br>`ping -c 5 8.8.8.8` - Send 5 packets and stop. |
| `ip` | The modern, all-in-one tool for network configuration. | `ip addr show` or `ip a` - Show IP addresses.<br>`ip route` - Show routing table.<br>`ip link` - Show network interfaces. |
| `ss` | A utility to investigate sockets (network connections). The modern replacement for `netstat`. | `sudo ss -tulnp` - Show listening TCP and UDP ports and the processes using them.<br>`ss -tp state established` - Show established TCP connections. |
| `traceroute` | Traces the network path (hops) to a destination host. | `traceroute google.com` |
| `nslookup` | Queries Internet name servers (DNS). | `nslookup example.com` |
| `dig` | A flexible tool for interrogating DNS name servers. More detailed than `nslookup`. | `dig google.com`<br>`dig MX google.com` - Get Mail eXchange records.<br>`dig @8.8.8.8 google.com` - Query a specific DNS server. |
| `curl` | A powerful tool for transferring data from or to a server, using various protocols. Great for testing APIs. | `curl http://example.com`<br>`curl -I http://example.com` - Fetch headers only.<br>`curl -L http://example.com` - Follow redirects. |
| `wget` | A non-interactive network downloader. Excellent for downloading files. | `wget https://example.com/file.zip`<br>`wget -c https://example.com/large-file.iso` - Continue a partial download. |
| `scp` | Securely copies files between hosts on a network over SSH. | `scp file.txt user@host:/remote/path/`<br>`scp user@host:/remote/file.txt /local/path/` |
| `ssh` | Provides secure shell access to remote machines. | `ssh user@hostname` |

---

## 8. Disk Management

Commands for managing disk space and filesystems.

| Command | Description | Example |
|---|---|---|
| `df` | Shows the amount of disk space used and available on filesystems. | `df -h` - Human-readable format.<br>`df -i` - Show inode usage. |
| `du` | Shows the disk space used by files and directories. | `du -sh /var/log` - Show a summary in human-readable format.<br>`du -ah . \| sort -hr \| head -n 10` - Find the top 10 largest files/dirs. |
| `fdisk` | A classic, powerful tool for manipulating disk partition tables. | `sudo fdisk -l` - List partition tables.<br>`sudo fdisk /dev/sda` - Interactively manage a disk. |
| `mount` | Mounts a filesystem to a directory in the file tree. | `sudo mount /dev/sdb1 /mnt/data` |
| `umount` | Unmounts a filesystem. | `sudo umount /mnt/data` |
| `lsblk` | Lists information about all available block devices (disks and partitions). | `lsblk`<br>`lsblk -f` - Show filesystem type and UUID. |
| `mkfs` | Creates a new filesystem on a device or partition. | `sudo mkfs.ext4 /dev/sdb1`<br>`sudo mkfs.xfs /dev/sdb1` |
| `fsck` | Checks and repairs a Linux filesystem. | `sudo fsck /dev/sdb1` |

---

## 9. System Monitoring & Performance

Commands for monitoring system resources and performance.

| Command | Description | Example |
|---|---|---|
| `top` | Displays a real-time view of system processes. | `top` |
| `htop` | An interactive, user-friendly process viewer. | `htop` |
| `vmstat` | Reports virtual memory statistics. | `vmstat 2 5` - Report every 2 seconds, 5 times. |
| `iostat` | Reports CPU and I/O statistics. | `iostat -d -x 2` - Show extended disk stats every 2 seconds. |
| `free` | Shows the amount of free and used memory in the system. | `free -h` - Human-readable format. |
| `uptime` | Shows how long the system has been running, number of users, and load average. | `uptime` |
| `uname` | Prints system information (kernel version, architecture, etc.). | `uname -a` - Show all information. |
| `hostname` | Shows or sets the system's hostname. | `hostname`<br>`sudo hostname new-name` |
| `dmesg` | Displays kernel ring buffer messages (boot messages, hardware errors). | `dmesg`<br>`dmesg -T` - Show human-readable timestamps. |
| `journalctl` | Queries the `systemd` journal (system logs). | `journalctl -f` - Follow live logs.<br>`journalctl -u nginx.service` - Show logs for a specific service.<br>`journalctl -b` - Show logs from the current boot. |

---

## 10. Text Processing

Commands for manipulating and processing text files. These are often chained together with pipes (`|`).

| Command | Description | Example |
|---|---|---|
| `cat` | Concatenates and displays files. | `cat file1.txt file2.txt > newfile.txt` |
| `less` | Views file content one screen at a time, with backward/forward navigation and search. | `less /var/log/syslog` |
| `head` | Shows the beginning lines of a file. | `head -n 20 file.txt` - Show the first 20 lines. |
| `tail` | Shows the ending lines of a file. | `tail -n 20 file.txt` - Show the last 20 lines.<br>`tail -f /var/log/syslog` - **f**ollow the file for live updates. |
| `grep` | Searches for text using patterns (regular expressions). | `grep "error" /var/log/nginx/error.log`<br>`grep -i "debug" app.log` - Case-**i**nsensitive search.<br>`grep -r "API_KEY" .` - **R**ecursive search in the current directory. |
| `sed` | A stream editor for filtering and transforming text. | `sed 's/old_text/new_text/g' file.txt` - Find and replace text. |
| `awk` | A powerful pattern scanning and processing language. | `awk '{print $1}' /var/log/nginx/access.log` - Print the first column of a file. |
| `cut` | Removes sections (columns) from each line of a file. | `cut -d':' -f1 /etc/passwd` - Get usernames from the password file. |
| `sort` | Sorts lines of text. | `sort names.txt`<br>`sort -r names.txt` - Reverse sort.<br>`sort -n numbers.txt` - Numeric sort. |
| `uniq` | Filters duplicate adjacent lines from a sorted file. | `sort names.txt \| uniq -c` - Count occurrences of each name. |
| `wc` | Prints the number of lines, words, and bytes in a file. | `wc -l file.txt` - Count lines only. |

---

## 11. Compression & Archiving

Commands for compressing and archiving files.

| Command | Description | Example |
|---|---|---|
| `tar` | Creates or extracts archive files (`.tar`, `.tar.gz`, `.tgz`). | `tar -cvf archive.tar /path/to/dir` - **c**reate a `.tar` archive.<br>`tar -xvf archive.tar` - **e**xtract a `.tar` archive.<br>`tar -czvf archive.tar.gz /path/to/dir` - Create and **z**ip with gzip.<br>`tar -xzvf archive.tar.gz` - Extract a gzipped archive. |
| `gzip` | Compresses files using gzip compression (`.gz`). | `gzip file.txt` -> `file.txt.gz` |
| `gunzip` | Decompresses `.gz` files. | `gunzip file.txt.gz` -> `file.txt` |
| `zip` / `unzip` | Packages and compresses (or extracts) files in the common `.zip` format. | `zip -r archive.zip /path/to/dir`<br>`unzip archive.zip` |
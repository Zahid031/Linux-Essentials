# All Commands

This document provides a comprehensive list of common Linux commands, categorized for easy reference. Each command includes a description of its function and an example of its usage.

---

## 1. Basic Commands

These are the fundamental commands for interacting with the Linux shell.

| Command | Description | Example |
|---|---|---|
| `ls` | Lists the contents of a directory. You can use `ls -l` for a detailed list. | `ls -l /home/user` |
| `cd` | Changes the current working directory. | `cd /home/user` |
| `pwd` | Prints the current working directory's full path. | `pwd` |
| `cp` | Copies files or directories from a source to a destination. | `cp file.txt backup.txt` |
| `mv` | Moves or renames files or directories. | `mv oldname.txt newname.txt` |
| `rm` | Removes (deletes) files or directories. Use `rm -r` for recursive deletion. | `rm file.txt` |
| `mkdir` | Creates a new directory. | `mkdir new_folder` |
| `rmdir` | Removes an empty directory. | `rmdir empty_folder` |
| `touch` | Creates an empty file or updates the timestamp of an existing file. | `touch newfile.txt` |
| `clear` | Clears the terminal screen. | `clear` |

---

## 2. User Management

Commands for managing user accounts and groups. Most of these require `sudo` privileges.

| Command | Description | Example (sudo for elevated privileges) |
|---|---|---|
| `useradd` | Adds a new user to the system. | `sudo useradd testusr` |
| `groupadd` | Creates a new user group. | `sudo groupadd testgrp` |
| `adduser` | An interactive script to add a new user. | `sudo adduser testusr` |
| `passwd` | Changes a user's password. | `passwd testusr` |
| `usermod` | Modifies a user account's properties (e.g., adding to a group). | `sudo usermod -aG sudo testusr` |
| `deluser` | Deletes a user from the system. | `sudo deluser testusr` |
| `userdel` | Deletes a user, including their home directory and mail spool. | `sudo userdel -r testusr` |
| `groupdel` | Deletes an existing user group. | `sudo groupdel testgrp` |
| `groups` | Shows the groups a user belongs to. | `groups testusrgrp` |
| `whoami` | Displays the current logged-in user. | `whoami` |
| `chage` | Changes user account aging information (e.g., password expiration). | `sudo chage -l testusr` |
| `who` | Shows who is currently logged on to the system. | `who` |
| `w` | Shows who is logged in and what they are doing. | `w` |
| `id` | Displays user ID (UID), group ID (GID), and group memberships. | `id testusr` |

---

## 3. File and Directory Management

Commands for finding and managing files and directories.

| Command | Description | Example |
|---|---|---|
| `find` | Searches for files in a directory hierarchy based on criteria. | `find /home/testusr -name "*.txt"` |
| `locate` | Finds files by name using a pre-built database. | `locate notes.txt` |
| `tree` | Displays directories in a tree-like format. | `tree directory_name` |
| `stat` | Displays detailed information about a file or filesystem. | `stat file.txt` |
| `basename` | Extracts the filename from a path. | `basename /home/testusr/file.txt` |
| `dirname` | Extracts the directory path from a file path. | `dirname /home/testusr/file.txt` |
| `rsync` | Synchronizes files and directories between two locations (local or remote). | `rsync -avz source/ destination/` |

---

## 4. File Permission & Ownership

Commands to control access to files and directories.

| Command | Description | Example |
|---|---|---|
| `chmod` | Changes file permissions using numeric (e.g., 755) or symbolic (e.g., u+x) values. | `chmod 755 script.sh` |
| `chown` | Changes the owner of a file or directory. | `sudo chown testusr:testusr file.txt` |
| `chgrp` | Changes the group ownership of a file or directory. | `sudo chgrp testgrp file.txt` |
| `umask` | Sets the default permissions for newly created files. | `umask 022` |

---

## 5. Process Management

Commands for managing running applications and processes.

| Command | Description | Example |
|---|---|---|
| `ps` | Displays a snapshot of currently running processes. | `ps aux` |
| `systemctl` | Controls the `systemd` system and service manager. | `systemctl start service_name` |
| `top` | Shows a real-time, dynamic view of running processes. | `top` |
| `kill` | Terminates a process by its Process ID (PID). | `kill -9 [PID]` |
| `killall` | Terminates processes by their name. | `killall firefox` |
| `htop` | An interactive process viewer (more user-friendly than `top`). | `htop` |
| `jobs` | Lists background jobs running in the current shell. | `jobs` |
| `bg` | Resumes a stopped job and runs it in the background. | `bg %1` |
| `fg` | Brings a background job to the foreground. | `fg %1` |
| `shutdown` | Shuts down the system. | `shutdown -h now` |
| `reboot` | Reboots the system. | `sudo reboot` |
| `poweroff` | Powers off the system. | `poweroff` |
| `init` | Changes the system runlevel (e.g., to shut down or reboot). | `init 0` |

---

## 6. Package Management

Commands for installing, updating, and removing software packages.

| Command | Description | Example |
|---|---|---|
| `apt` | A high-level package manager for Debian-based systems (e.g., Ubuntu). | `apt update` |
| `apt-get` | A command-line tool for handling packages on Debian-based systems. | `apt-get install package_name` |
| `yum` | A package manager for Red Hat-based systems (e.g., CentOS, Fedora). | `yum install package_name` |

---

## 7. Network Commands

Commands for configuring and troubleshooting network connections.

| Command | Description | Example |
|---|---|---|
| `ping` | Tests connectivity to a network host. | `ping google.com` |
| `ifconfig` | Configures network interfaces (deprecated in favor of `ip`). | `ifconfig eth0` |
| `ip` | Shows and manipulates routing, devices, and network interfaces. | `ip a` |
| `netstat` | Displays network connections, routing tables, and interface statistics (deprecated). | `netstat -tuln` |
| `traceroute` | Traces the route that packets take to a network host. | `traceroute google.com` |
| `nslookup` | Queries Internet name servers interactively. | `nslookup example.com` |
| `dig` | A flexible tool for interrogating DNS name servers. | `dig example.com` |
| `ss` | A utility to investigate sockets (a modern replacement for `netstat`). | `ss -tuln` |
| `curl` | Transfers data from or to a server, using various protocols. | `curl http://example.com` |
| `wget` | Downloads files from the internet. | `wget http://example.com/file.zip` |
| `scp` | Securely copies files between hosts on a network. | `scp file.txt user@host:/tmp/` |
| `ssh` | Provides secure shell access to remote machines. | `ssh user@192.168.1.10` |
| `ftp` | A client for the File Transfer Protocol. | `ftp ftp.example.com` |
| `nc` | A versatile utility for reading from and writing to network connections. | `nc -l -p 844` |

---

## 8. Disk Management

Commands for managing disk space and filesystems.

| Command | Description | Example |
|---|---|---|
| `df` | Shows the amount of disk space used and available on filesystems. | `df -h` |
| `du` | Shows the disk space used by files and directories. | `du -sh /home/testusr` |
| `fdisk` | A dialog-driven program for creating and manipulating partition tables. | `sudo fdisk /dev/sda` |
| `mount` | Mounts a filesystem to a directory. | `mount /dev/sdb1 /mnt` |
| `umount` | Unmounts a filesystem. | `umount /mnt` |
| `fstab` | The configuration file for mounting filesystems at boot time. | `/etc/fstab` |
| `lsblk` | Lists information about all available block devices. | `lsblk` |
| `blkid` | Displays attributes of block devices. | `blkid` |
| `parted` | A program for manipulating disk partitions. | `sudo parted /dev/sda` |
| `mkfs` | Creates a new filesystem on a device or partition. | `sudo mkfs.ext4 /dev/sdb1` |
| `fsck` | Checks and repairs a Linux filesystem. | `sudo fsck /dev/sdb1` |

---

## 9. System Monitoring & Performance

Commands for monitoring system resources and performance.

| Command | Description | Example |
|---|---|---|
| `top` | Displays a real-time view of system processes. | `top` |
| `htop` | An interactive process viewer. | `htop` |
| `vmstat` | Reports virtual memory statistics. | `vmstat 1` |
| `iostat` | Reports CPU and I/O statistics. | `iostat` |
| `free` | Shows the amount of free and used memory in the system. | `free -h` |
| `time` | Measures the execution time of a program. | `time ls` |
| `uptime` | Shows how long the system has been running. | `uptime` |
| `uname` | Prints system information (kernel version, etc.). | `uname -a` |
| `hostname` | Shows or sets the system's hostname. | `hostname` |
| `dmesg` | Displays boot and system messages. | `dmesg` |
| `lscpu` | Displays CPU architecture information. | `lscpu` |

---

## 10. Security & Permissions

Commands related to system security and user permissions.

| Command | Description | Example |
|---|---|---|
| `sudo` | Executes a command as another user (typically the root user). | `sudo apt install nginx` |
| `su` | Switches to another user account. | `su - testusr` |
| `visudo` | Safely edits the `/etc/sudoers` file. | `sudo visudo` |
| `sudoers` | Manages sudo access for users. | `sudo visudo -f /etc/sudoers` |
| `gpasswd` | Administers group passwords. | `gpasswd -a testusr testgrp` |
| `ufw` | A simple firewall utility. | `sudo ufw enable` |
| `iptables` | Configures packet filtering rules for the kernel firewall. | `sudo iptables -L` |

---

## 11. Help and Documentation

Commands for getting help and reading documentation.

| Command | Description | Example |
|---|---|---|
| `man` | Displays the manual page for a command. | `man ls` |
| `info` | Displays command information in GNU format. | `info coreutils` |
| `--help` | Shows usage information and options for a command. | `ls --help` |
| `whatis` | Provides a brief, one-line description of a command. | `whatis grep` |
| `apropos` | Searches the manual pages for a keyword. | `apropos copy` |

---

## 12. Text Processing

Commands for manipulating and processing text files.

| Command | Description | Example |
|---|---|---|
| `cat` | Concatenates and displays files. | `cat file.txt` |
| `less` | Views file content one screen at a time, with backward navigation. | `less file.txt` |
| `more` | A basic pager for viewing file content. | `more file.txt` |
| `head` | Shows the beginning lines of a file. | `head file.txt` |
| `tail` | Shows the ending lines of a file. Use `tail -f` to follow a file. | `tail file.txt` |
| `grep` | Searches for text using patterns (regular expressions). | `grep 'hello' file.txt` |
| `vi` or `vim` | Powerful and widely-used text editors. | `vi script.sh` |
| `sed` | A stream editor for filtering and transforming text. | `sed 's/old/new/g' file.txt` |
| `awk` | A pattern scanning and processing language. | `awk '{print $1}' file.txt` |
| `cut` | Removes sections from each line of a file. | `cut -d':' -f1 /etc/passwd` |
| `sort` | Sorts lines of text. | `sort names.txt` |
| `uniq` | Filters duplicate lines from a sorted file. | `uniq sorted.txt` |
| `tr` | Translates or deletes characters. | `tr 'a-z' 'A-Z' < file.txt` |
| `wc` | Prints the number of lines, words, and bytes in a file. | `wc file.txt` |

---

## 13. Compression & Archiving

Commands for compressing and archiving files.

| Command | Description | Example |
|---|---|---|
| `tar` | Creates or extracts archive files. Flags: `-c` (create), `-x` (extract), `-v` (verbose), `-z` (gzip), `-f` (file). | `tar -czvf archive.tar.gz file.txt` |
| `gzip` | Compresses files using gzip compression. | `gzip file.txt` |
| `gunzip` | Decompresses `.gz` files. | `gunzip file.txt.gz` |
| `bzip2` | Compresses files using bzip2 compression. | `bzip2 file.txt` |
| `zip` | Compresses files into a `.zip` archive. | `zip archive.zip file.txt` |
| `unzip` | Extracts files from a `.zip` archive. | `unzip archive.zip` |

---

## 14. Scheduling & Automation

Commands for scheduling and automating tasks.

| Command | Description | Example |
|---|---|---|
| `cron` | A daemon that executes scheduled commands. | (Runs automatically in the background) |
| `crontab` | Manages cron jobs for a user. Use `crontab -e` to edit. | `crontab -e` |
| `at` | Schedules a one-time job to run at a specific time. | `echo 'shutdown now' | at 10:00` |
| `batch` | Runs commands when the system load is low. | `echo './script.sh' | batch` |
| `sleep` | Delays execution for a specified amount of time. | `sleep 10s` |

---

## 15. System Monitoring

A summary of essential system monitoring commands.

| Command | Description | Example |
|---|---|---|
| `top` | Provides a real-time, dynamic view of system processes. | `top` |
| `htop` | An interactive and more user-friendly process viewer. | `htop` |
| `vmstat` | Reports virtual memory statistics. | `vmstat 1` |
| `iostat` | Reports CPU and I/O statistics. | `iostat` |
| `free` | Shows memory usage. | `free -h` |
| `uptime` | Shows how long the system has been running. | `uptime` |
| `lscpu` | Displays CPU architecture information. | `lscpu` |
| `lsusb` | Lists USB devices. | `lsusb` |
| `lspci` | Lists PCI devices. | `lspci` |
| `lshw` | Lists hardware configuration. | `sudo lshw` |
| `dmesg` | Displays kernel ring buffer messages. | `dmesg | less` |
| `journalctl` | Views logs collected by `systemd`. | `journalctl -xe` |
| `watch` | Executes a command periodically, showing the output in real-time. | `watch -n 1 free` |

---

## 16. Shell & Scripting

Commands related to shell scripting and environment variables.

| Command | Description | Example |
|---|---|---|
| `bash` | The GNU Bourne Again SHell, a common command-line interpreter. | `bash script.sh` |
| `sh` | The basic shell interpreter. | `sh script.sh` |
| `source` | Executes a script in the current shell. | `source ~/.bashrc` |
| `./` | A shortcut for `source` to run a script. | `./script.sh` |
| `export` | Sets environment variables. | `export PATH=$PATH:/home/testusr/bin` |
| `alias` | Creates command shortcuts. | `alias ll='ls -la'` |
| `read` | Reads user input from the terminal. | `read name` |
| `echo` | Displays a line of text. | `echo 'Hello, testusr!'` |

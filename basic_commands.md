# Linux Basic Commands: A Quick Reference

This file provides a curated list of essential Linux commands for beginners and those needing a quick refresher. The commands are grouped by category for easy navigation.

### Table of Contents
1.  [File System Navigation](#1-file-system-navigation)
2.  [File & Directory Manipulation](#2-file--directory-manipulation)
3.  [Viewing File Content](#3-viewing-file-content)
4.  [Searching for Files & Text](#4-searching-for-files--text)
5.  [User & Permissions Management](#5-user--permissions-management)
6.  [System Information & Monitoring](#6-system-information--monitoring)
7.  [Networking](#7-networking)
8.  [Archiving & Compression](#8-archiving--compression)
9.  [Package Management](#9-package-management)

---

## 1. File System Navigation

Commands to find your way around the Linux file system.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `pwd` | **P**rint **W**orking **D**irectory | `pwd` |
| `ls` | **L**i**s**t directory contents | `ls -lah` (lists in **l**ong format, **a**ll files, with **h**uman-readable sizes) |
| `cd` | **C**hange **D**irectory | `cd /var/log` (absolute path) <br> `cd ../project` (relative path) <br> `cd ~` (go to home directory) <br> `cd -` (go to previous directory) |

---

## 2. File & Directory Manipulation

Commands for creating, deleting, and organizing files and directories.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `touch` | Create a new, empty file or update its timestamp | `touch new-file.txt` |
| `mkdir` | **M**a**k**e a new **dir**ectory | `mkdir my-project` <br> `mkdir -p project/src/main` (creates parent directories as needed) |
| `cp` | **C**o**p**y a file or directory | `cp source.txt destination.txt` <br> `cp -r source_directory/ destination_directory/` (**r**ecursive for directories) |
| `mv` | **M**o**v**e or rename a file or directory | `mv old-name.txt new-name.txt` (rename) <br> `mv report.pdf ./archive/` (move) |
| `rm` | **R**e**m**ove a file | `rm temp-file.log` <br> `rm -i important.txt` (**i**nteractive, prompts before deleting) |
| `rmdir` | **R**e**m**ove an *empty* directory | `rmdir empty-folder` |
| `rm -r` | **R**e**m**ove a directory and its contents | `rm -r old-project/` (**r**ecursive) <br> > **Warning:** `rm -rf` is extremely powerful and deletes everything without confirmation. Use with extreme caution. |

---

## 3. Viewing File Content

Commands to inspect the contents of files without a graphical editor.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `cat` | Con**cat**enate and display the full content of a file | `cat config.yml` |
| `less` | View file content one page at a time (interactive) | `less large-logfile.log` (use arrow keys to navigate, `q` to quit) |
| `head` | Display the first few lines of a file | `head -n 20 file.txt` (shows the first 20 lines) |
| `tail` | Display the last few lines of a file | `tail -n 20 file.txt` (shows the last 20 lines) <br> `tail -f /var/log/syslog` (**f**ollows the file for live updates) |

---

## 4. Searching for Files & Text

Tools to find files or specific content within files.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `grep` | **G**lobal **R**egular **E**xpression **P**rint (search text) | `grep "error" /var/log/nginx/error.log` <br> `grep -ir "api_key" .` (**i**gnore case, **r**ecursive search in current dir) |
| `find` | Search for files in a directory hierarchy | `find /home/user -name "*.py"` (finds all Python files in a user's home) <br> `find . -type f -mtime -7` (finds **f**iles modified in the last **7** days) |

---

## 5. User & Permissions Management

Commands related to users and file access rights.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `whoami` | Show the current effective username | `whoami` |
| `sudo` | **S**uper **U**ser **Do** (execute a command as another user, typically root) | `sudo apt update` |
| `chmod` | **Ch**ange file **mod**e (permissions) | `chmod +x script.sh` (adds e**x**ecute permission for all users) <br> `chmod 755 script.sh` (sets permissions using octal: rwx for **u**ser, r-x for **g**roup & **o**thers) |
| `chown` | **Ch**ange file **own**er and group | `sudo chown user:group file.txt` |

---

## 6. System Information & Monitoring

Commands to check the health and status of your system.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `uname -a` | Print all system information (kernel, hostname, etc.) | `uname -a` |
| `df -h` | **D**isk **F**ree (show disk space usage) | `df -h` (**h**uman-readable format) |
| `du -sh` | **D**isk **U**sage (show file or directory space usage) | `du -sh /var/log` (**s**ummary, **h**uman-readable) |
| `free -h` | Show free and used system memory | `free -h` (**h**uman-readable format) |
| `top` | Display and manage running processes in real-time | `top` |

---

## 7. Networking

Basic commands for inspecting network connectivity.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `ping` | Check network connectivity to a host | `ping google.com` (press `Ctrl+C` to stop) |
| `ip` | Show and manipulate routing, devices, and addresses | `ip addr show` (shows IP addresses for all interfaces) <br> `ip route` (shows the routing table) |

---

## 8. Archiving & Compression

Commands for bundling and compressing files.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `tar` | **T**ape **Ar**chiver (bundle files into a `.tar` archive) | `tar -czvf archive.tar.gz /path/to/dir` (**c**reate, g**z**ip, **v**erbose, **f**ile) <br> `tar -xzvf archive.tar.gz` (e**x**tract, g**z**ip, **v**erbose, **f**ile) |
| `zip` / `unzip` | Package and compress (or extract) files in `.zip` format | `zip -r archive.zip my_folder/` <br> `unzip data.zip` |

---

## 9. Package Management

Commands to install, update, and remove software. The command varies by Linux distribution.

| Distro Family | Command | Example Usage |
| :--- | :--- | :--- |
| Debian / Ubuntu | `apt` | `sudo apt update && sudo apt upgrade` <br> `sudo apt install htop` <br> `sudo apt remove htop` |
| Red Hat / CentOS / Fedora | `yum` / `dnf` | `sudo dnf check-update` <br> `sudo dnf install htop` <br> `sudo dnf remove htop` |

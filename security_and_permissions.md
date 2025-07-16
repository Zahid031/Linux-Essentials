# Linux Security & Permissions for DevOps

Securing Linux systems is fundamental to DevOps. This guide covers the core concepts of file permissions, user management, and basic system hardening.

### Table of Contents
1.  [Standard File Permissions](#1-standard-file-permissions)
2.  [Changing Ownership & Permissions](#2-changing-ownership--permissions)
3.  [User & Group Management](#3-user--group-management)
4.  [Elevating Privileges with `sudo`](#4-elevating-privileges-with-sudo)
5.  [Advanced Permissions: ACLs](#5-advanced-permissions-acls)
6.  [Basic Firewall Management](#6-basic-firewall-management)

---

## 1. Standard File Permissions

The foundation of Linux security. When you run `ls -l`, you see permissions like `-rwxr-xr--`.

*   The first character indicates the file type (`-` for file, `d` for directory).
*   The next nine characters are three sets of three:
    1.  **User (Owner):** Permissions for the file's owner.
    2.  **Group:** Permissions for members of the file's group.
    3.  **Other (World):** Permissions for everyone else.
*   Each set defines **r**ead, **w**rite, and e**x**ecute permissions.

| Permission | Octal Value | Meaning for Files | Meaning for Directories |
| :--- | :--- | :--- | :--- |
| `r` (read) | 4 | Can view the file's contents. | Can list the contents of the directory (`ls`). |
| `w` (write) | 2 | Can modify or delete the file. | Can create, delete, or rename files within the directory. |
| `x` (execute) | 1 | Can run the file (if it's a script/program). | Can enter the directory (`cd`). |

---

## 2. Changing Ownership & Permissions

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `chmod` | **Ch**ange file **mod**e (permissions). | `chmod 755 script.sh`: Sets `rwx` for owner, `r-x` for group/other (7=4+2+1, 5=4+1). <br> `chmod +x script.sh`: Adds e**x**ecute permission for everyone. <br> `chmod -R 644 /var/www/html`: **R**ecursively sets `rw-r--r--` on all files in a directory. |
| `chown` | **Ch**ange file **own**er and group. | `sudo chown www-data:www-data config.php`: Changes owner and group. <br> `sudo chown -R user:group /opt/app`: **R**ecursively change ownership for a directory. |
| `chgrp` | **Ch**ange **gr**ou**p** ownership. | `sudo chgrp developers /var/log/app.log` |

---

## 3. User & Group Management

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `useradd` | Add a new user account. | `sudo useradd -m -s /bin/bash newuser`: Creates a user with a **m**ake-home directory and sets their default shell. |
| `usermod` | Modify an existing user account. | `sudo usermod -aG sudo newuser`: **A**ppends the user to the `sudo` **G**roup. |
| `userdel` | Delete a user account. | `sudo userdel -r olduser`: Deletes the user and their home di**r**ectory. |
| `groupadd` | Add a new group. | `sudo groupadd developers` |
| `passwd` | Change a user's password. | `sudo passwd newuser` |

---

## 4. Elevating Privileges with `sudo`

`sudo` allows a permitted user to execute a command as the superuser or another user.

*   Configuration is managed in the `/etc/sudoers` file.
*   **Always** edit this file using `sudo visudo`, which performs a syntax check before saving to prevent locking yourself out.

**Example `/etc/sudoers` entry:**
```
# Allow members of the 'devops' group to run any command
%devops ALL=(ALL:ALL) ALL

# Allow the 'deploy' user to restart the nginx service without a password
deploy ALL=(ALL) NOPASSWD: /usr/sbin/systemctl restart nginx
```

---

## 5. Advanced Permissions: ACLs

**A**ccess **C**ontrol **L**ists (ACLs) provide a more flexible permission model than the standard `rwx` for user/group/other. They let you grant permissions to specific, additional users or groups.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `setfacl` | **Set** **F**ile **ACL**s. | `setfacl -m u:bob:rwx /path/to/file`: Gives user `bob` read, write, and execute permissions. <br> `setfacl -R -m g:developers:rwx /srv/app`: **R**ecursively gives the `developers` group `rwx` permissions. |
| `getfacl` | **Get** **F**ile **ACL**s. | `getfacl /path/to/file`: Shows the standard permissions plus any special ACL entries. |

---

## 6. Basic Firewall Management

A firewall is the first line of defense. `ufw` (Uncomplicated Firewall) is a user-friendly frontend for `iptables`.

| Command | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `ufw` | **U**ncomplicated **F**ire**w**all. | `sudo ufw status verbose`: Check the firewall status. <br> `sudo ufw enable`: Enable the firewall. <br> `sudo ufw allow ssh`: Allow incoming traffic for SSH (port 22). <br> `sudo ufw allow 8080/tcp`: Allow incoming traffic on a specific TCP port. <br> `sudo ufw deny 3306`: Deny incoming traffic on a port. |

## 1. Navigating from the Command Line:

| **Command** | **Action** | **Remark / Syntax / Example** |
|-------------|------------|------------------------------|
| `pwd` | Show current location in absolute path format | Displays the current directory path. Example: `pwd` |
| `ls` | List contents (files and directories) of the current location | Common options: `-a`, `-l`, `-h`. Example: `ls -lah` |
| `cd` | Change directory | Example: `cd /var/www`, `cd ..` (go back a directory), `cd ~` (go to home directory) |

---
---

## 2. Managing Operating System from the Command Line:

| **Command** | **Action** | **Remark / Syntax / Example** |
|-------------|------------|------------------------------|
| `top` or `htop` | Show machine specifications, tasks, and resource utilization | Similar to Task Manager in Windows. Example: `top` |
| `systemctl` | Manage system services | Example: `systemctl status nginx`, `systemctl start nginx`, `systemctl stop nginx` |
| `apt`, `yum`, `snap` | Install, update, list, and remove packages | Example: `apt install nginx`, `apt update` |

---
---

## 3. Basic File Operations:

| **Command** | **Action** | **Remark / Syntax / Example** |
|-------------|------------|------------------------------|
| `cp` | Make a copy of the file | Example: `cp /var/www/myapp.conf ~/` |
| `mv` | Move or rename a file | Example: `mv /var/log/app.log /home/user/` |
| `rm` | Delete a file | Example: `rm -v myfile.txt`, `rm -rvf mydir` |
| `touch` | Create a blank file | Example: `touch file.txt` |
| `mkdir` | Create a directory | Example: `mkdir -p ~/mydir` |
| `chown` | Change ownership | Example: `chown yaman:yaman myfile` |
| `chmod` | Change file permissions | Example: `chmod 400 myscript.sh`, `chmod +wx myscript.sh` |
| `cat` | Concatenate and display file contents | Example: `cat myscript.sh` |
| `wc` | Show word or line count | Example: `wc -l myscript.sh`, `wc -w myscript.sh` |
| `vim`, `nano` | Command-line text editors | Example: `vim myscript.sh`, `nano myscript.sh` |

---
---

## 4. Permissions and User Management:

| **Command** | **Action** | **Remark / Syntax / Example** |
|-------------|------------|------------------------------|
| `chmod` (Symbolic) | Change file permissions using symbolic notation | Example: `chmod u+x myscript.sh` |
| `chmod` (Numeric) | Change file permissions using numeric codes | Example: `chmod 750 myscript.sh` |
| `whoami` | Display current logged-in username | Example: `whoami` |
| `/etc/passwd` | View user information | Example: `cat /etc/passwd` |
| `/etc/group` | View group information | Example: `cat /etc/group` |

---
---

## 5. Basic System Administration Tasks:

| **Command** | **Action** | **Remark / Syntax / Example** |
|-------------|------------|------------------------------|
| `df` | Show information about file system (disks and partitions) | Example: `df -h` |
| `du` | Display disk usage of directories | Example: `du -sh ~/` |
| `ncdu` | A newer tool for improved directory size analysis | Example: `ncdu -x ~` |

---
---
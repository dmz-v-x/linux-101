## Basic Linux Commands and System Information

### 1. Checking Your Linux Distribution

Before using commands, it’s helpful to know **which Linux distribution** you are running (like Ubuntu, Fedora, Debian, etc.).

---

### `cat /etc/os-release`

**Purpose:** Displays detailed information about your Linux distribution.

**Example:**
```bash
cat /etc/os-release
```

**Output:**
```
NAME="Ubuntu"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
ID=ubuntu
PRETTY_NAME="Ubuntu 22.04.3 LTS"
```

---

### `lsb_release -a`

**Purpose:** Another command to show distribution info (if `lsb_release` is installed).

**Example:**
```bash
lsb_release -a
```

**Output:**
```
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.3 LTS
Release:        22.04
Codename:       jammy
```

---

### `hostnamectl`

**Purpose:** Shows OS details, kernel version, and hostname.

**Example:**
```bash
hostnamectl
```

**Output:**
```
Static hostname: ubuntu-server
Operating System: Ubuntu 22.04.3 LTS
Kernel: Linux 5.15.0-89-generic
Architecture: x86-64
```

---

### 2. Basic File and Directory Commands

These are the most frequently used commands to navigate and manage your Linux file system.

---

### `pwd` — Print Working Directory

**Purpose:** Shows your current directory path.

```bash
pwd
```

**Output:**
```
/home/alex
```

---

### `ls` — List Files and Directories

| Command | Description |
|----------|--------------|
| `ls` | Lists files in the current directory |
| `ls -a` | Lists **all files**, including hidden ones (starting with `.`) |
| `ls -l` | Lists files in **long format** (shows permissions, owner, size, date) |
| `ls -al` | Combines `-a` and `-l` (all + long format) |

**Example:**
```bash
ls -al
```

**Output:**
```
drwxr-xr-x  3 alex alex 4096 Oct 16 11:20 .
drwxr-xr-x  5 root root 4096 Oct 16 10:55 ..
-rw-r--r--  1 alex alex  220 Oct  5 10:00 .bashrc
-rw-r--r--  1 alex alex   50 Oct  6 09:45 notes.txt
```

---

### `cd` — Change Directory

**Examples:**
```bash
cd /home          # Move to /home directory
cd ..             # Move up one level
cd ~              # Move to home directory
cd -              # Switch to previous directory
```

---

### `mkdir` — Make Directory

**Purpose:** Creates a new directory.

```bash
mkdir projects
```

Creates a folder named `projects` in the current directory.

---

### `rmdir` — Remove Empty Directory

**Purpose:** Deletes **empty** directories only.

```bash
rmdir old_folder
```

If the folder isn’t empty, use `rm -r` instead.

---

### `rm` — Remove Files or Directories

| Command | Description |
|----------|--------------|
| `rm <file>` | Removes a file |
| `rm -r <directory>` | Removes directory **recursively** (use with caution!) |

**Examples:**
```bash
rm notes.txt
rm -r old_project
```

> **Warning:** `rm -r` permanently deletes files — there’s no recycle bin.

---

### `touch` — Create Empty Files or Update Timestamp

**Example:**
```bash
touch test.txt
```

Creates a blank file `test.txt` or updates its timestamp if it already exists.

---

### `cp` — Copy Files or Directories

| Command | Description |
|----------|--------------|
| `cp <source> <destination>` | Copy a file |
| `cp -r <source> <destination>` | Copy a directory recursively |

**Examples:**
```bash
cp file1.txt backup/
cp -r /home/alex/docs /home/alex/backup/
```

---

### `mv` — Move or Rename Files

**Examples:**
```bash
mv file1.txt /home/alex/docs/        # Move file
mv oldname.txt newname.txt           # Rename file
```

---

### 3. Viewing File Contents

### `cat` — Display File Contents

**Example:**
```bash
cat notes.txt
```

Shows entire file content at once.

---

### `more` — View File One Page at a Time

**Example:**
```bash
more /etc/passwd
```

Press:
- `Space` → Next page  
- `q` → Quit  

---

### 4. System and Resource Commands

### `lshw` — Display Hardware Information

**Purpose:** Shows detailed hardware specs (CPU, RAM, disks, etc.).

**Example:**
```bash
sudo lshw | less
```

**Sample Output:**
```
*-cpu
   product: Intel(R) Core(TM) i5-8265U
   capacity: 3900MHz
*-memory
   size: 8GiB
```

---

### `free -h` — Display Memory Usage

**Example:**
```bash
free -h
```

**Output:**
```
              total        used        free      shared  buff/cache   available
Mem:           7.7G        2.1G        3.6G        150M        2.0G        5.2G
Swap:          2.0G        0B          2.0G
```

`-h` = human-readable (MB/GB)

---

### 5. User and Privilege Commands

### `sudo <command>` — Run Command as Root

**Example:**
```bash
sudo apt update
```

Prompts for your password and executes with admin privileges.

---

### `su` — Switch User

**Examples:**
```bash
su                 # Switch to root user (requires root password)
su - username      # Switch to another user
```

To exit the session:
```bash
exit
```

---

### 6. Command History and Shortcuts

### `history` — Show Command History

**Examples:**
```bash
history          # Shows all commands with line numbers
history 20       # Shows last 20 commands
!45              # Re-runs command number 45
```

---

###  Keyboard Shortcuts

| Shortcut | Action |
|-----------|---------|
| `Ctrl + C` | Stop/interrupt a running command |
| `Ctrl + Shift + V` | Paste text into terminal |
| `Ctrl + R` | Search previous commands in history |
| `Ctrl + L` | Clear terminal screen |

---

### 7. Summary Table

| Command | Description |
|----------|--------------|
| `cat /etc/os-release` | View OS info |
| `lsb_release -a` | View distribution details |
| `hostnamectl` | View system and kernel info |
| `pwd` | Print current directory |
| `ls`, `ls -a`, `ls -l`, `ls -al` | List files |
| `cd`, `cd ..`, `cd ~`, `cd -` | Change directories |
| `mkdir` | Create directory |
| `rmdir`, `rm`, `rm -r` | Remove files/directories |
| `touch` | Create or update file |
| `cp`, `cp -r` | Copy files/directories |
| `mv` | Move or rename files |
| `cat`, `more` | View file contents |
| `lshw` | Hardware info |
| `free -h` | Memory usage |
| `sudo` | Run as administrator |
| `su` | Switch user |
| `history`, `history <n>` | Command history |
| `Ctrl+C`, `Ctrl+Shift+V` | Keyboard shortcuts |

---

### 8. Practice Tip

You can create a **test workspace** to safely experiment:

```bash
mkdir ~/practice
cd ~/practice
touch file1.txt file2.txt
mkdir backup
cp file1.txt backup/
ls -al
```

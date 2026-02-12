## Users, Groups, and File Permissions in Linux

In Linux, everything is treated as a **file** — whether it’s a text file, a directory, or even a device.  
Access to these files is controlled using **permissions**, **users**, and **groups**.

---

### 1. What Are Users in Linux?

Every person or service that uses the Linux system is a **user**.  
Each user is identified by a unique number called a **UID (User ID)**.

There are two main types of users:

| Type | Description | UID Range |
|------|--------------|-----------|
| **System Users** | Used by background services (like `mysql`, `daemon`, `www-data`) | `0 – 999` |
| **Regular Users** | Created for humans (e.g., `john`, `alex`) | `1000+` |

---

### 👑 The Root User

- The **root** user is the **superuser** — UID `0`.
- Has permission to do **anything** on the system.
- Uses the `#` prompt symbol.
- Be very careful when logged in as root.

---

### 2. What Are Groups?

A **group** is a collection of users that share the same permissions for certain files or directories.  
Each group has a unique **GID (Group ID)**.

**Example:**
- The group `developers` can include users `alex`, `sam`, and `lisa`.
- If a file belongs to the `developers` group, all members can access it according to the group permissions.

Each user belongs to:
1. A **primary group** (assigned when the user is created).
2. Optionally, one or more **secondary groups**.

---

### Viewing User and Group Information

**View current user:**
```bash
whoami
```

**View all users:**
```bash
cat /etc/passwd
```

**View all groups:**
```bash
cat /etc/group
```

**Check your user details:**
```bash
id
```

**Example Output:**
```
uid=1000(alex) gid=1000(alex) groups=1000(alex),27(sudo),1001(developers)
```

---

### 3. File Types in Linux

Linux represents different types of files using **symbols** at the start of the permission string.

| Symbol | Type | Example |
|---------|------|----------|
| `-` | Regular file | `-rw-r--r--` |
| `d` | Directory | `drwxr-xr-x` |
| `l` | Symbolic link | `lrwxrwxrwx` |
| `b` | Block device (e.g., hard drive) | `brw-rw----` |
| `c` | Character device (e.g., keyboard, mouse) | `crw-rw----` |
| `s` | Socket (for inter-process communication) | `srwxrwxrwx` |
| `p` | Named pipe | `prw-r--r--` |

---

### 4. Understanding File Permissions

Each file or directory has **permissions** that control **who can read, write, or execute** it.

The permission string looks like this:

```
-rwxrwxrwx
```

It has **10 characters** broken down into parts:

| Position | Meaning | Example |
|-----------|----------|---------|
| 1 | File type (`-`, `d`, `l`, etc.) | `-` (regular file) |
| 2–4 | **Owner permissions** | `rwx` |
| 5–7 | **Group permissions** | `rwx` |
| 8–10 | **Others permissions** | `rwx` |

---

### What `r`, `w`, and `x` Mean

| Symbol | Permission | Meaning for Files | Meaning for Directories |
|---------|-------------|------------------|--------------------------|
| `r` | Read | View contents | List files inside |
| `w` | Write | Edit or delete the file | Add or delete files inside |
| `x` | Execute | Run as program/script | Enter or traverse directory |

---

### Example 1: Regular File

```
-rwxr-xr--
```

| Type | Owner | Group | Others |
|------|--------|--------|--------|
| `-` (regular file) | `rwx` (read, write, execute) | `r-x` (read, execute) | `r--` (read only) |

✅ **Meaning:**  
- The owner can read, write, and execute.  
- Group members can read and execute.  
- Others can only read.

---

### Example 2: Directory

```
drwxrwxr-x
```

| Type | Owner | Group | Others |
|------|--------|--------|--------|
| `d` (directory) | `rwx` | `rwx` | `r-x` |

✅ **Meaning:**  
- Owner and group can list, add, and remove files.  
- Others can only open and list contents.

---

### 5. Ownership in Linux

Each file and directory is “owned” by:
- A **user** (the creator or assigned owner)
- A **group**

You can check ownership using:
```bash
ls -l
```

**Example Output:**
```
-rw-r--r-- 1 alex developers 1234 Oct 18 10:00 notes.txt
```

| Field | Meaning |
|--------|----------|
| `alex` | File owner |
| `developers` | Group owner |
| `-rw-r--r--` | Permission string |

---

### 6. Changing Ownership and Permissions

### Change Ownership

**Syntax:**
```bash
sudo chown <user>:<group> <file>
```

**Example:**
```bash
sudo chown alex:developers notes.txt
```

---

### Change Permissions

**Syntax:**
```bash
chmod <permissions> <file>
```

**Example (symbolic):**
```bash
chmod u+x script.sh      # Give execute to user
chmod g-w file.txt       # Remove write for group
chmod o-r file.txt       # Remove read for others
```

**Example (numeric):**
- Each permission has a number value:
  - `r = 4`, `w = 2`, `x = 1`

Add them up for each group:
| Owner | Group | Others | Permissions | Numeric |
|--------|--------|--------|--------------|----------|
| rwx | r-x | r-- | Read, write, execute for owner | 755 |

```bash
chmod 755 script.sh
```

---

### 7. Numeric (Octal) Permission Representation

| Permission | Binary | Decimal | Meaning |
|-------------|---------|----------|----------|
| --- | 000 | 0 | No permissions |
| --x | 001 | 1 | Execute only |
| -w- | 010 | 2 | Write only |
| -wx | 011 | 3 | Write + Execute |
| r-- | 100 | 4 | Read only |
| r-x | 101 | 5 | Read + Execute |
| rw- | 110 | 6 | Read + Write |
| rwx | 111 | 7 | Read + Write + Execute |

Example:
```
chmod 644 notes.txt
```
✅ Owner: `rw-` (read, write)  
✅ Group: `r--` (read only)  
✅ Others: `r--` (read only)

---

### 8. Special Permission Bits (Advanced)

Linux has three **special permissions** that modify default behavior:

| Symbol | Name | Purpose |
|---------|------|----------|
| `s` | SetUID | Run as the file’s owner |
| `s` | SetGID | Run as the file’s group |
| `t` | Sticky Bit | Only file owner can delete (used in `/tmp`) |

**Example:**
```
-rwsr-xr-x
```
- The `s` replaces the owner’s execute (`x`), meaning SetUID is enabled.

---

### 9. Summary of Key Concepts

| Concept | Description |
|----------|--------------|
| **User** | Identified by UID; owns files and processes |
| **Group** | Collection of users with shared permissions |
| **System User** | Used by background services (UID 0–999) |
| **Regular User** | Created for people (UID 1000+) |
| **File Type** | `-` (file), `d` (dir), `l` (link), etc. |
| **Permissions** | `r` (read), `w` (write), `x` (execute) |
| **Ownership** | Defined by user and group |
| **chmod** | Change permissions |
| **chown** | Change ownership |
| **ls -l** | View ownership and permissions |
| **/etc/passwd** | Lists all users |
| **/etc/group** | Lists all groups |

---

### 10. Practical Example

```bash
# Create a directory
mkdir project

# Create a file
touch project/code.sh

# Check permissions
ls -l project

# Give execute permission to user
chmod u+x project/code.sh

# Change group to 'developers'
sudo chown alex:developers project/code.sh

# Verify
ls -l project
```

**Output:**
```
-rwxr--r-- 1 alex developers 0 Oct 18 11:00 code.sh
```

✅ Owner (`alex`): read, write, execute  
✅ Group (`developers`): read  
✅ Others: read  

---

> In short:  
> **Users** own files,  
> **Groups** share access,  
> **Permissions** control what each can do,  
> and **ownership + permissions together** define Linux’s secure multi-user environment.

---

# Changing File Permissions in Linux

## 1. What Are File Permissions?

Every file or directory in Linux has **three levels of access control**:

| Category | Description |
|-----------|--------------|
| **Owner (User)** | The person who owns the file (usually who created it) |
| **Group** | Other users who are part of the file’s group |
| **Others** | Everyone else on the system |

Each of these can have **three types of permissions**:

| Symbol | Permission | Description |
|---------|-------------|-------------|
| `r` | Read | View file contents or list files in a directory |
| `w` | Write | Modify or delete the file (or add/remove files in a directory) |
| `x` | Execute | Run as a program/script or enter a directory |

---

### Example: Viewing Permissions

Run:
```bash
ls -l
```

Output:
```
-rwxr-xr--
```

| Section | Description |
|----------|--------------|
| `-` | Regular file |
| `rwx` | Owner can read, write, execute |
| `r-x` | Group can read, execute |
| `r--` | Others can read only |

---

## 2. The `chmod` Command (Change Mode)

The command to **change file permissions** is `chmod`.

**Syntax:**
```bash
chmod [options] [permissions] [file or directory]
```

Example:
```bash
chmod 755 script.sh
```

---

## 3. Ways to Change Permissions

There are **two main methods** to change permissions in Linux:

1. **Symbolic Mode** → Uses letters and symbols (`u+`, `g-`, etc.)
2. **Numeric (Octal) Mode** → Uses numbers (`755`, `644`, etc.)

Both achieve the same goal but work differently.

---

## 4. Symbolic Mode

Symbolic mode uses letters to represent **who** you’re changing permissions for and **what** permissions to add, remove, or set.

### Structure

```
chmod [who][operator][permission] file
```

---

### Who (Target)

| Symbol | Meaning |
|---------|----------|
| `u` | User (file owner) |
| `g` | Group |
| `o` | Others |
| `a` | All (user + group + others) |

---

### Operator

| Symbol | Meaning |
|---------|----------|
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exact permission (replace existing) |

---

### Permissions

| Symbol | Meaning |
|---------|----------|
| `r` | Read |
| `w` | Write |
| `x` | Execute |

---

### Symbolic Mode Examples

| Command | Meaning |
|----------|----------|
| `chmod u+x file.sh` | Add execute for the file owner |
| `chmod g-w file.txt` | Remove write from group |
| `chmod o+r notes.txt` | Give read permission to others |
| `chmod a+r file.txt` | Give everyone read access |
| `chmod u=rwx,g=rx,o=r file.txt` | Set exact permissions for all categories |

---

### Combine Multiple Operations

You can make multiple changes at once:
```bash
chmod u+x,g-w,o+r myfile.txt
```

Adds execute for user  
Removes write from group  
Adds read for others  

---

## 5. Numeric (Octal) Mode

In numeric mode, permissions are represented using numbers.

| Permission | Binary | Value |
|-------------|---------|--------|
| `r` | 100 | 4 |
| `w` | 010 | 2 |
| `x` | 001 | 1 |

You **add up** the values for each permission group (user, group, others).

---

### Example Breakdown

```
-rwxr-xr--
```

| Category | Permissions | Value |
|-----------|--------------|--------|
| Owner | rwx | 7 (4+2+1) |
| Group | r-x | 5 (4+0+1) |
| Others | r-- | 4 (4+0+0) |

So this file = **`755`**

---

### Common Numeric Permissions

| Code | Permissions | Description |
|------|--------------|--------------|
| `777` | rwxrwxrwx | Everyone can do everything (not secure) |
| `755` | rwxr-xr-x | Owner full, others read + execute |
| `700` | rwx------ | Only owner can access |
| `644` | rw-r--r-- | Owner read/write, others read-only |
| `600` | rw------- | Private file (owner only) |
| `444` | r--r--r-- | Read-only file |
| `000` | ---------- | No permissions for anyone |

---

### Numeric Mode Examples

```bash
chmod 755 script.sh      # Owner full, others read/execute
chmod 644 notes.txt      # Owner read/write, others read
chmod 700 secret.txt     # Only owner can access
chmod 600 config.json    # Private configuration file
chmod 777 test.log       # Everyone can read/write/execute (not recommended)
```

---

## 6. Changing Permissions for Directories

Permissions apply differently for directories:

| Permission | Meaning (for Directories) |
|-------------|--------------------------|
| `r` | Allows listing the files inside |
| `w` | Allows creating or deleting files |
| `x` | Allows entering (`cd`) the directory |

Example:
```bash
chmod 755 my_folder
```

Owner → Full access  
Group/Others → Can view and enter, but not modify  

---

## 7. Recursive Permission Changes

To apply permission changes to a directory **and all its files/subdirectories**:

```bash
chmod -R 755 /path/to/directory
```

- `-R` stands for **recursive**.
- Applies permissions to everything inside the specified directory.

---

## 8. Checking Permissions

Use `ls -l` to verify changes:
```bash
ls -l
```

Example output:
```
-rwxr-xr--
```

To check permissions numerically:
```bash
stat -c "%A %a %n" *
```

Output:
```
-rw-r--r-- 644 notes.txt
-rwxr-xr-x 755 script.sh
```

---

## 9. Combining Ownership and Permissions

Sometimes you’ll need to change both **ownership** and **permissions** together.

```bash
sudo chown username:groupname file.txt
chmod 640 file.txt
```

Now `username` owns the file and has read/write access  
Group has read-only access  
Others have no access  

---

## 10. Practical Examples

**Example 1: Give execute permission to owner**
```bash
chmod u+x run.sh
```

**Example 2: Remove write permission for others**
```bash
chmod o-w myfile.txt
```

**Example 3: Make directory readable by everyone**
```bash
chmod -R a+r my_project/
```

**Example 4: Make a private file**
```bash
chmod 600 secrets.txt
```

---

## 11. Summary Table

| Command | Description |
|----------|--------------|
| `chmod u+x file` | Add execute for owner |
| `chmod g-w file` | Remove write for group |
| `chmod o+r file` | Add read for others |
| `chmod a+r file` | Add read for everyone |
| `chmod u=rwx,g=rx,o=r file` | Set specific permissions |
| `chmod 755 file` | rwxr-xr-x |
| `chmod 644 file` | rw-r--r-- |
| `chmod 700 file` | rwx------ |
| `chmod -R 755 dir` | Apply to directory + contents |

---

## 12. Tips & Best Practices

-  Always follow the **principle of least privilege** (only give necessary access).
-  Avoid using `chmod 777` — it gives everyone full control.
-  Verify permissions after each change using `ls -l`.
-  Use `-R` carefully — recursive changes can modify system-critical files.
-  Use numeric mode for scripts, symbolic mode for small adjustments.

---

## 13. Recap

| Concept | Description |
|----------|--------------|
| `chmod` | Command to change permissions |
| Symbolic Mode | Uses letters (`u+`, `g-`, etc.) |
| Numeric Mode | Uses numbers (`755`, `644`, etc.) |
| Directory Permissions | Control read/write/enter access |
| Recursive | Apply changes to all files inside |
| Verification | Use `ls -l` or `stat` to confirm |

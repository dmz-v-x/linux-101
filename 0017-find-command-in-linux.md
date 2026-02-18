# `find` Command in Linux

## 1. What is `find`?

The `find` command searches for files and directories in a **directory hierarchy** — starting from a given location and applying filters or actions.

**Syntax:**
```bash
find [path] [options] [expression]
```

**Example:**
```bash
find /home/user -name "file.txt"
```
This searches for a file named `file.txt` inside `/home/user` and all its subdirectories.

---

## 2. Basic Search by Name

### Case-Sensitive Search
```bash
find /home/user -name "file.txt"
```
Finds all files exactly named **file.txt** (case-sensitive).

### Case-Insensitive Search
```bash
find /home/user -iname "file.txt"
```
Finds all files named `file.txt`, `File.txt`, `FILE.TXT`, etc.

---

## 3. Search by File Type

Use the `-type` option to specify what you’re looking for:

| Type | Description |
|-------|-------------|
| `f` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `b` | Block device |
| `c` | Character device |
| `s` | Socket |
| `p` | Named pipe |

**Examples:**
```bash
find /home/user -type f       # All files
find /home/user -type d       # All directories
```

---

## 4. Search by File Size

Use the `-size` option to find files based on their size.

| Operator | Meaning |
|-----------|----------|
| `+` | Greater than |
| `-` | Less than |
| none | Exactly equal |

**Examples:**
```bash
find /home/user -size +100M   # Files larger than 100MB
find /home/user -size -10k    # Files smaller than 10KB
find /home/user -size 1G      # Files exactly 1GB
```

**Units you can use:**
- `c` = bytes  
- `k` = kilobytes  
- `M` = megabytes  
- `G` = gigabytes  

---

## 5. Search by Time (Modification, Access, Change)

You can find files based on their **modification time (`-mtime`)**, **access time (`-atime`)**, or **change time (`-ctime`)**.

| Flag | Meaning |
|-------|----------|
| `-mtime` | Modified time |
| `-atime` | Last accessed time |
| `-ctime` | Metadata change time |

### Examples:
```bash
find /home/user -mtime -7     # Modified in the last 7 days
find /home/user -atime +30    # Not accessed in the last 30 days
find /home/user -ctime -1     # Changed in the last 24 hours
```

**Note:**
- `+n` → older than *n* days  
- `-n` → within last *n* days  
- `n` → exactly *n* days ago  

---

## 6. Search by Owner or Group

You can search for files based on their **owner** or **group**.

**Examples:**
```bash
find /home/user -user john         # Files owned by user 'john'
find /home/user -group developers  # Files belonging to 'developers' group
```

---

## 7. Combining Multiple Conditions

You can use logical operators:
- `-and` or `-a` (both must be true)
- `-or` or `-o` (either condition)
- `!` or `-not` (negation)

**Example:**
```bash
find /home/user -type f -name "*.log" -and -size +10M
```
→ Finds `.log` files larger than 10MB.

```bash
find / -type f \( -name "*.jpg" -o -name "*.png" \)
```
→ Finds files with either `.jpg` **or** `.png` extensions.

---

## 8. Executing Commands on Found Files

The real power of `find` comes from the `-exec` option, which allows you to **run commands** on each file found.

**Syntax:**
```bash
find [path] [conditions] -exec [command] {} \;
```

Where:
- `{}` = placeholder for each found file.
- `\;` = indicates the end of the command.

### Examples:

#### Find and Delete
```bash
find /home/user -type f -name "*.tmp" -exec rm -f {} \;
```
→ Deletes all `.tmp` files.

#### Find and Search Text
```bash
find /home/user -type f -exec grep -l "error" {} \;
```
→ Prints names of files containing the word "error".

#### Find and Move
```bash
find /downloads -type f -name "*.jpg" -exec mv {} /pictures/ \;
```
→ Moves all `.jpg` files to `/pictures`.

---

## 9. Recursive Delete with Pattern Match

You can delete specific files safely using `find`:
```bash
find /home/user -type f -name "*.txt" -exec rm -f {} \;
```
This deletes all `.txt` files under `/home/user`.

---

## 10. Real-World Use Cases

| Task | Command |
|------|----------|
| Find files modified in last 2 days | `find /var/log -mtime -2` |
| Find and delete old log files | `find /var/log -name "*.log" -mtime +30 -exec rm {} \;` |
| Find all empty directories | `find /home/user -type d -empty` |
| Find all `.conf` files and list permissions | `find /etc -name "*.conf" -exec ls -l {} \;` |
| Find large files (>1GB) | `find / -type f -size +1G` |

---

## 11. Optimization — Avoid “Permission Denied”

When searching system-wide (`/`), you might see permission errors.  
Use `2>/dev/null` to suppress them:

```bash
find / -name "file.txt" 2>/dev/null
```

---

## 12. Recap

| Concept | Description | Example |
|----------|--------------|----------|
| Search by name | Matches filenames | `find /home -name "test.txt"` |
| Case-insensitive search | Ignores case | `find /home -iname "test.txt"` |
| Search by type | File or directory | `find /home -type d` |
| Search by size | By file size | `find /home -size +100M` |
| Search by modification time | Based on days | `find /home -mtime -7` |
| Search by user/group | Ownership filter | `find /home -user john` |
| Execute command | Perform actions | `find /home -exec rm {} \;` |

---

## 13. Key Takeaways

- `find` recursively searches directories based on **conditions**.
- It can **filter by name, type, size, time, or ownership**.
- Combine filters using **AND / OR / NOT**.
- Use **`-exec`** to run operations on matched files.
- Suppress errors with **`2>/dev/null`**.
- It’s ideal for **automation scripts**, **cleanup jobs**, and **system auditing**.

---

> **Remember:**  
> `find` is like the Swiss army knife of file searching — it doesn’t just locate files,  
> it helps you **take action** on them right away.

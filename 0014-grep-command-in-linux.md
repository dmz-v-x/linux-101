# `grep` Command in Linux

## 1. What is `grep`?

**`grep`** stands for **Global Regular Expression Print**.

In simple terms, it:
- **Searches** for text patterns inside files or output.
- **Prints** the matching lines.
- Supports **regular expressions** (regex) for advanced pattern matching.

---

## 2. Basic Syntax

```bash
grep [options] "pattern" [file...]
```

**Example:**
```bash
grep "error" logfile.txt
```
This searches for the word **“error”** inside the file `logfile.txt` and prints all matching lines.

---

## 3. Real-World Example

Imagine you have a log file `/var/log/syslog` and want to find all lines mentioning “network”:

```bash
grep "network" /var/log/syslog
```

Output:
```
Oct 19 10:22:14 systemd: Starting Network Manager...
Oct 19 10:22:15 systemd: Network Manager started.
```

---

## 4. Common `grep` Options

Here are the most useful and common options used with `grep`:

| Option | Meaning | Example | Description |
|--------|----------|----------|-------------|
| `-i` | Ignore case | `grep -i "hello" file.txt` | Matches “Hello”, “HELLO”, or “hello”. |
| `-w` | Match whole words only | `grep -w "cat" file.txt` | Matches “cat” but **not** “catalog” or “scatter”. |
| `-n` | Show line numbers | `grep -n "main()" program.c` | Useful for locating exact line positions. |
| `-c` | Count matching lines | `grep -c "error" logfile.txt` | Returns how many lines contain “error”. |
| `-v` | Invert match | `grep -v "success" logfile.txt` | Shows lines that **do not** contain “success”. |
| `-r` or `-R` | Recursive search | `grep -r "TODO" /home/user/projects` | Searches through all subdirectories. |
| `-l` | Show only filenames | `grep -l "pattern" *.txt` | Lists files that contain the pattern. |
| `-L` | Show files without matches | `grep -L "pattern" *.txt` | Lists files that **don’t** contain the pattern. |
| `--color=auto` | Highlight matches | `grep --color=auto "error" logfile.txt` | Highlights search terms in color. |

---

## 5. Pattern Matching Examples

### Case-Sensitive Search (Default)
```bash
grep "hello" file.txt
```

### Case-Insensitive Search
```bash
grep -i "hello" file.txt
```

### Search Whole Word
```bash
grep -w "hello" file.txt
```

### Search Beginning of Line
```bash
grep "^start" file.txt
```
Matches lines that **begin** with “start”.

### Search End of Line
```bash
grep "end$" file.txt
```
Matches lines that **end** with “end”.

### Show Line Numbers
```bash
grep -n "error" logfile.txt
```

### Count Matches
```bash
grep -c "error" logfile.txt
```

### Search Recursively in Directory
```bash
grep -r "pattern" /path/to/directory
```

---

## 6. Using Wildcards

You can search in multiple files using wildcards:
```bash
grep "pattern" *.txt
```
Searches for `"pattern"` in all `.txt` files in the current directory.

---

## 7. Combining with Other Commands (Pipes)

You can **pipe (`|`)** another command’s output into `grep`.

Example — find processes related to “apache”:
```bash
ps aux | grep "apache"
```

Example — find “root” users in `/etc/passwd`:
```bash
cat /etc/passwd | grep "root"
```

Example — filter network connections for “ssh”:
```bash
netstat -tuln | grep "ssh"
```

---

## 8. Searching Multiple Files

```bash
grep "main" file1.c file2.c file3.c
```

The output will show the filename and line where the match occurs:
```
file1.c:23:int main() {
file2.c:17:int main() {
```

---

## 9. Recursive Search in All Files

```bash
grep -r "TODO" /home/user/projects
```
Searches for “TODO” in all files and subdirectories under `/home/user/projects`.

---

## 10. Counting and Summarizing Matches

Count occurrences of a word:
```bash
grep -c "error" logfile.txt
```

Count total occurrences (not just lines):
```bash
grep -o "error" logfile.txt | wc -l
```

---

## 11. Advanced Usage — Regex (Regular Expressions)

`grep` supports **regular expressions** for pattern matching.

### Match any character
```bash
grep "b.t" file.txt
```
Matches “bat”, “bit”, “bot”.

### Match digits
```bash
grep "[0-9]" file.txt
```

### Match specific range
```bash
grep "^[A-Z]" file.txt
```
Matches lines starting with a **capital letter**.

---

## 12. Extended `grep` Versions

There are three main `grep` variants:

| Command | Description |
|----------|--------------|
| `grep` | Basic regular expression support |
| `egrep` or `grep -E` | Extended regex (supports `+`, `?`, `|`, etc.) |
| `fgrep` or `grep -F` | Fixed string search (faster, no regex) |

Example using extended regex:
```bash
grep -E "cat|dog" pets.txt
```

---

## 13. Example Scenarios

### Find all failed SSH logins
```bash
grep "Failed password" /var/log/auth.log
```

### Find all users starting with “a” in `/etc/passwd`
```bash
grep "^a" /etc/passwd
```

### Find all `.conf` files that contain “port”
```bash
grep -r "port" /etc/*.conf
```

### Find word count of "error" in logs
```bash
grep -o "error" /var/log/syslog | wc -l
```

---

## 14. Summary Table

| Task | Command | Description |
|------|----------|-------------|
| Search text | `grep "pattern" file.txt` | Basic search |
| Case-insensitive | `grep -i "pattern" file.txt` | Ignores case |
| Whole word | `grep -w "word" file.txt` | Matches full words |
| Show line numbers | `grep -n "pattern" file.txt` | Show matching line numbers |
| Count matches | `grep -c "pattern" file.txt` | Number of matching lines |
| Recursive search | `grep -r "pattern" /dir` | Search subdirectories |
| Search multiple files | `grep "pattern" *.log` | Search across many files |
| Invert match | `grep -v "pattern" file.txt` | Show lines without match |
| Highlight results | `grep --color=auto "pattern" file.txt` | Color highlight matches |

---

## Final Recap

- `grep` searches for text patterns.
- Works with files **and command outputs**.
- Supports **regular expressions** for flexible searching.
- Common flags: `-i`, `-w`, `-r`, `-n`, `-c`, `--color`.
- Variants: `grep`, `egrep`, `fgrep`.

---

> **Tip:**  
> You’ll use `grep` everywhere — from debugging logs to finding config values, filtering system output, or scripting automation.

**In short:**  
`grep` is the **Swiss Army knife** of text searching in Linux.

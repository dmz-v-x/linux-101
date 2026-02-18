# Pipe (`|`) in Linux

## 1. What is a Pipe?

A **pipe (`|`)** connects two or more commands together.  
The **output (stdout)** of the first command becomes the **input (stdin)** for the next command.

**Syntax:**
```bash
command1 | command2 | command3 ...
```

Each command does **one specific job**, and when connected using `|`, they work together to perform powerful tasks.

---

## 2. Basic Example

```bash
ls | grep "file"
```

Explanation:
- `ls` lists all files in the current directory.
- The pipe `|` takes that list and sends it as input to `grep`.
- `grep "file"` filters only lines containing the word `"file"`.

So the final output shows **only files with “file” in their name**.

---

## 3. Why Use Pipes?

Pipes help you:
- Combine small commands to perform **complex tasks**.
- Avoid creating temporary files.
- Save time and memory.
- Work with **real-time data streams** (like logs or command outputs).

**Think of it like this:**
> Each command is a worker in a factory.  
> A pipe connects one worker’s output to the next worker’s input.

---

## 4. Common Examples

### Example 1 — Filtering Files
```bash
ls | grep "log"
```
Shows only files containing “log”.

---

### Example 2 — Searching Within a File
```bash
cat file.txt | grep "error"
```
Reads the file using `cat` and passes it to `grep`, which filters lines containing “error”.

---

### Example 3 — Sorting and Counting Unique Lines
```bash
cat file.txt | grep "error" | sort | uniq -c
```

Explanation step-by-step:
1. `cat file.txt` → outputs file content.
2. `grep "error"` → filters lines containing “error”.
3. `sort` → sorts those lines.
4. `uniq -c` → removes duplicates and counts occurrences.

This command chain gives a **count of unique error lines** in a file.

---

### Example 4 — Combining with Error Redirection
```bash
ls | grep "file" 2> error.log
```

Explanation:
- The pipe connects `ls` → `grep`.
- Any **error messages (stderr)** are redirected to `error.log` using `2>`.
- So normal results appear on screen, and errors are logged separately.

---

## 5. How Pipes Work Internally

When you use a pipe:
1. The **shell creates two processes** — one for each command.
2. The **stdout** of the first command is **connected** to the **stdin** of the next.
3. They run **simultaneously**, with the kernel managing data transfer between them.

So instead of saving the output to a temporary file, it happens **in-memory** and **instantly**.

---

## 6. Combining Multiple Commands

You can chain as many commands as you want:

```bash
ps aux | grep "apache" | awk '{print $1, $2}' | sort | uniq
```

Explanation:
1. `ps aux` → shows all running processes.
2. `grep "apache"` → filters Apache-related processes.
3. `awk '{print $1, $2}'` → prints user and process ID.
4. `sort | uniq` → sorts and removes duplicates.

This outputs a **clean list of Apache process owners and PIDs**.

---

## 7. Useful Commands Often Used with Pipes

| Command | Use Case | Example |
|----------|-----------|----------|
| `grep` | Search/filter lines | `cat file.txt | grep "error"` |
| `sort` | Sort output | `ls | sort` |
| `uniq` | Remove duplicates | `sort file.txt | uniq` |
| `wc` | Count lines/words | `cat file.txt | wc -l` |
| `head` | Show first few lines | `cat file.txt | head -n 5` |
| `tail` | Show last few lines | `cat file.txt | tail -n 10` |
| `awk` | Field extraction | `ps aux | awk '{print $1}'` |
| `cut` | Split fields | `cat file.txt | cut -d':' -f1` |
| `tee` | Save and pass output | `ls | tee files.txt | grep "log"` |

---

## 8. Real-World Examples

### Example 1 — Find Running Processes by User
```bash
ps aux | grep "alice"
```

### Example 2 — View Only Open Ports
```bash
netstat -tuln | grep "LISTEN"
```

### Example 3 — Monitor Logs in Real Time
```bash
tail -f /var/log/syslog | grep "error"
```

### Example 4 — Find Word Count of a Keyword
```bash
cat file.txt | grep "hello" | wc -l
```

---

## 9. Difference Between Pipe (`|`) and Redirection (`>`)

| Concept | Symbol | Function |
|----------|----------|----------|
| **Pipe** | `\|` | Sends output of one command **to another command** |
| **Redirection** | `>` | Sends output of a command **to a file** |


**Example:**

- Pipe:
  ```bash
  cat file.txt | grep "error"
  ```
  → output goes to `grep`.

- Redirection:
  ```bash
  cat file.txt > errors.txt
  ```
  → output goes to `errors.txt`.

---


## 10. Combining Pipe and Redirection

You can use both together for powerful workflows:

```bash
cat file.txt | grep "error" | sort | uniq -c > error_summary.txt
```

Output is filtered, sorted, counted, and then redirected into a file.

---

## 11. Recap

| Task | Command | Description |
|------|----------|-------------|
| Basic pipe | `ls | grep "file"` | Send output of one command to another |
| Chain multiple commands | `cat file.txt | grep "error" | sort` | Filter → sort → display |
| Pipe + append error log | `ls | grep "file" 2> error.log` | Pipe with error redirection |
| Count matching lines | `cat file.txt | grep "error" | wc -l` | Filter and count |
| Save piped output | `cat file.txt | grep "error" > result.txt` | Pipe + redirect |

---

## 12. Key Takeaways

- A **pipe** sends **stdout** of one command to **stdin** of another.
- You can chain multiple commands for **powerful workflows**.
- Use with commands like `grep`, `sort`, `uniq`, `awk`, `wc`, `head`, and `tail`.
- Combine with **redirection (`>`, `>>`)** for logging and data processing.

---

> **Remember:**  
> Linux follows the **philosophy of small, modular tools**.  
> Each command does one job — and **pipes** connect them to form something greater.

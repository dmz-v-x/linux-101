# Redirection in Linux

## 1. What is Redirection?

Normally when you run a command, there are three standard data streams involved:

| Stream | Number | Description | Default Target |
|---------|---------|--------------|----------------|
| **stdin** | 0 | Standard Input | Keyboard |
| **stdout** | 1 | Standard Output | Terminal (Screen) |
| **stderr** | 2 | Standard Error | Terminal (Screen) |

**Redirection** allows you to change where these streams go or come from.

For example:
- You can redirect command output (stdout) to a **file**.
- You can redirect errors (stderr) to a **log file**.
- You can read input (stdin) from a **file** instead of typing.

---

## 2. Basic Output Redirection (`>`)

The `>` operator **redirects** the **output (stdout)** of a command to a file.

```bash
echo "Hello, World!" > output.txt
```

- Creates (or replaces) `output.txt` with the text “Hello, World!”
- If the file already exists, it will be **overwritten**.

Example:
```bash
ls /etc > list.txt
```
The list of `/etc` directory files is stored in `list.txt` instead of being displayed on screen.

---

## 3. Appending Output (`>>`)

The `>>` operator **appends** output to the end of an existing file **without overwriting** it.

```bash
echo "Hi again!" >> output.txt
```
Adds the new line “Hi again!” to `output.txt`, keeping existing data.

**Example use case:**
```bash
date >> system.log
```
Appends current timestamp to `system.log` each time it runs.

---

## 4. Redirecting Errors (`2>`)

By default, **error messages** (like “file not found”) are printed to **stderr (2)**, not stdout.

You can redirect errors using:
```bash
ls non_existent_file 2> error.log
```
The error message is saved into `error.log`.

If you run the same command again with `2>>`, it will **append** to the file:
```bash
ls non_existent_file 2>> error.log
```

---

## 5. Redirecting Both Output and Error

Sometimes, you want to **capture both** normal output and errors in a single file.

```bash
ls /etc non_existent_file > result.txt 2>&1
```

Explanation:
- `> result.txt` redirects **stdout (1)** to `result.txt`.
- `2>&1` means “send stderr (2) to wherever stdout (1) is going” — i.e., the same file.

Now, both **results and errors** go into `result.txt`.

---

### Shortcut (Newer Syntax)
A simpler modern syntax:
```bash
ls /etc non_existent_file &> result.txt
```
Equivalent to `> result.txt 2>&1`

To append both outputs:
```bash
ls /etc non_existent_file &>> result.txt
```

---

## 6. Input Redirection (`<`)

You can **feed input** to a command from a file instead of typing manually.

Example:
```bash
cat < input.txt
```
`cat` reads the content of `input.txt` (as if you typed it).

Another example:
```bash
sort < names.txt
```
Sorts the contents of `names.txt` and displays the sorted list.

---

## 7. Combining Input and Output Redirection

You can combine input and output redirections in the same command.

Example:
```bash
sort < unsorted.txt > sorted.txt
```
Reads input from `unsorted.txt` and writes sorted result into `sorted.txt`.

---

## 8. Summary of File Descriptors

| Descriptor | Symbol | Description |
|-------------|----------|-------------|
| `0` | `<` | Input (stdin) |
| `1` | `>` | Output (stdout) |
| `2` | `2>` | Error (stderr) |

---

## 9. Practical Examples

| Task | Command | Description |
|------|----------|-------------|
| Redirect output to file | `echo "data" > file.txt` | Overwrites file |
| Append output | `echo "data" >> file.txt` | Adds at end |
| Redirect errors | `ls xyz 2> error.txt` | Saves errors |
| Redirect both output & error | `ls /etc xyz > result.txt 2>&1` | Combine both streams |
| Redirect input | `cat < file.txt` | Read from file |
| Combine input/output | `sort < file.txt > sorted.txt` | Sort contents |

---

## 10. Advanced Use Cases

### Redirect both stdout and stderr to different files
```bash
ls /etc non_existent_file > out.log 2> err.log
```
Output goes to `out.log`, errors go to `err.log`.

### Redirect only stderr to the screen, stdout to a file
```bash
ls /etc non_existent_file > result.txt 2>&1 | grep "error"
```

### Create or empty a file quickly
```bash
> empty.txt
```
Creates an empty file (or clears it if it exists).

---

## 11. Real-World Scenarios

### Logging command output
```bash
ping -c 4 google.com > ping.log
```
Saves ping results in a log file.

### Logging errors only
```bash
backup.sh 2> backup_errors.log
```

### Storing both outputs in a log
```bash
./deploy.sh > deploy.log 2>&1
```

---

## 12. Recap

| Symbol | Purpose | Behavior |
|---------|----------|-----------|
| `>` | Redirect stdout | Overwrites |
| `>>` | Redirect stdout | Appends |
| `2>` | Redirect stderr | Overwrites |
| `2>>` | Redirect stderr | Appends |
| `<` | Redirect stdin | Reads input from file |
| `2>&1` | Merge stderr into stdout | Combine both |
| `&>` | Redirect both stdout & stderr | Shorter syntax |

---

## Final Tip

Redirection is one of the **most powerful** Linux concepts.  
It’s heavily used in:
- Shell scripting
- Logging
- Error handling
- Data processing

Once you understand these three streams — `stdin`, `stdout`, `stderr` — you can **control the entire data flow** of your shell environment like a pro.

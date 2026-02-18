# SUID, SGID and the Sticky Bit 

## Quick summary

- **SUID (Set User ID, value 4)** — when set on an executable, the program runs with the file **owner**’s privileges (often root).  
- **SGID (Set Group ID, value 2)** — when set on a **directory**, new files/subdirs inherit the directory’s **group**; when set on an executable, the program runs with the file **group**’s privileges.  
- **Sticky bit (value 1)** — when set on a **directory**, only the **file owner**, the **directory owner** or **root** can delete/rename files inside it (useful for `/tmp`).

Numeric leading digit: add `4` for SUID, `2` for SGID, `1` for sticky.  
Examples: `4755` (SUID + 755), `2755` (SGID + 755), `1777` (sticky + 777).

---

## How these bits appear in `ls -l`

Permission string example:
```
-rwsr-xr-x  # SUID set on an executable (s in user execute position)
drwxrwsr-x  # SGID set on a directory (s in group execute position)
drwxrwxrwt  # Sticky bit set on a directory (t in others execute position)
```

- `s` in user position: SUID (replaces `x` for owner if execute present).  
- `s` in group position: SGID (replaces `x` for group).  
- `t` in others position: Sticky bit (replaces `x` for others).

You can also see the numeric (octal) mode with `stat`:
```bash
stat -c "%A %a %n" somefile
# -rwsr-xr-x 4755 /usr/bin/example
```

---

## 1) SUID — Set User ID (bit value 4)

### What it does (simple)
When an executable has **SUID** set, anyone who runs that program temporarily gets the **effective user ID** of the file **owner** for the duration of that program. This allows unprivileged users to perform specific tasks that normally require the owner’s privileges.

### Real-world example
- `/usr/bin/passwd` is typically **SUID root**.  
  - `passwd` needs to update `/etc/shadow` (owned by root). When you run `passwd` as a normal user, the program runs with **effective UID = root** just long enough to write your encrypted password into the protected file.

### How to set SUID
Symbolic:
```bash
sudo chmod u+s /usr/bin/myapp
```
Numeric:
```bash
sudo chmod 4755 /usr/bin/myapp
```
(Here `4` is SUID + `755` permissions for owner/group/others.)

### How to remove SUID
Symbolic:
```bash
sudo chmod u-s /usr/bin/myapp
```
Numeric (reset leading bit):
```bash
sudo chmod 0755 /usr/bin/myapp
```

### How to check for SUID files
Search system for SUID files:
```bash
find / -perm /4000 -type f 2>/dev/null
```

### Example walkthrough: create a test SUID program (educational)
> **Do not** set SUID on arbitrary scripts in production — it's a serious security risk. Use binary C programs for tests and understand the risks.

1. Write a small C program that prints EUID/UID:
```c
/* suid-test.c */
#include <stdio.h>
#include <unistd.h>
int main(){
    printf("uid=%d euid=%d\n", getuid(), geteuid());
    return 0;
}
```
2. Compile and install:
```bash
gcc suid-test.c -o suid-test
sudo mv suid-test /usr/local/bin/
sudo chown root:root /usr/local/bin/suid-test
sudo chmod 4755 /usr/local/bin/suid-test
```
3. Run as normal user:
```bash
./suid-test
# Output: uid=1001 euid=0   (shows program runs with euid=root)
```

### Security notes (SUID)
- SUID programs are powerful and often targeted by attackers.  
- Avoid SUID on scripts (kernels often ignore SUID on interpreted scripts).  
- Only trusted, well-audited binaries should be SUID.  
- Regularly audit SUID files (use `find / -perm /4000`).

---

## 2) SGID — Set Group ID (bit value 2)

SGID behaves differently depending on whether it's on a **file (executable)** or a **directory**.

### A) SGID on an executable (file)
- When set on a program file, the program runs with the **effective group ID** of the file’s group while it executes.
- Use case: a program that needs access to resources owned by a particular group.

Set SGID on a binary:
```bash
sudo chmod g+s /usr/local/bin/someprogram
# or numeric:
sudo chmod 2755 /usr/local/bin/someprogram
```
Check:
```bash
ls -l /usr/local/bin/someprogram
# -rwxr-sr-x 1 root devs ...
```

### B) SGID on a directory (most common use)
- When SGID is set on a **directory**, files and subdirectories **created inside** inherit the **group** of the directory (not the primary group of the creating user). This is great for shared workspaces.

#### Real-world collaboration example (step-by-step)
Objective: `/shared` directory where `alice` and `bob` collaborate; both should be able to edit each others' files via shared group `devs`.

1. Create shared dir and group:
```bash
sudo mkdir /shared
sudo groupadd devs
```

2. Set group ownership of `/shared` and add users to the group:
```bash
sudo chgrp devs /shared
sudo usermod -aG devs alice
sudo usermod -aG devs bob
```

3. Set SGID bit and reasonable permissions:
```bash
sudo chmod 2755 /shared
# 2 = SGID, 7 = owner rwx, 5 = group r-x, 5 = others r-x
```

4. Behavior now:
- If **alice** creates `/shared/file1`, the file’s group will be `devs`.
- **bob** (as member of `devs`) can access that file according to group permissions.

5. You can make group-writable so collaborators edit each other’s files:
```bash
sudo chmod 2775 /shared   # group has write permission as well
```

6. Verify:
```bash
touch /shared/alice.txt    # as alice
ls -l /shared
# -rw-rw-r-- 1 alice devs 0 ... alice.txt
```

#### Notes about default file permissions (umask)
- Even with SGID, the **group write** permission depends on the file mode and the user’s `umask`. If you want newly created files to be group-writable, ensure either:
  - Users set their `umask` (e.g., `002` so new files are group-writable), or
  - Use a directory creation policy (scripts/ACLs) to adjust permissions.

Set a collaborative `umask` in `~/.bashrc`:
```bash
umask 002   # new files: rw-rw-r-- ; new dirs: rwxrwxr-x
```

#### How to set SGID recursively (use with care)
```bash
sudo chmod -R g+s /shared
```

#### Find SGID files/directories
```bash
find / -perm /2000 -print 2>/dev/null
```

---

## 3) Sticky Bit — (bit value 1)

### What it does (simple)
When **sticky bit** is set on a **directory**, files in that directory can only be **deleted or renamed** by:
- the file **owner**, or
- the **directory owner**, or
- **root**.

This prevents users from deleting other users’ files in a shared directory even if the directory is world-writable.

### Real-world example: `/tmp`
- `/tmp` is world-writable so everyone can create files, but without sticky bit anyone could delete others’ files.
- `/tmp` has the sticky bit set (mode `1777`):
```bash
ls -ld /tmp
# drwxrwxrwt 14 root root 4096 Oct 18 /tmp
```
Note the `t` at the end and numeric `1777`.

### How to set sticky bit
Symbolic:
```bash
sudo chmod +t /shared
```
Numeric:
```bash
sudo chmod 1777 /shared
```

### How to remove sticky bit
```bash
sudo chmod -t /shared
# or
sudo chmod 0777 /shared
```

### Example workflow: make a shared drop directory where people cannot delete each others' files
```bash
sudo mkdir /var/drop
sudo chmod 1777 /var/drop
# Users can write files, but cannot remove files they don't own.
```

### Find directories with sticky bit
```bash
find / -type d -perm /1000 -print 2>/dev/null
```

---

## Interpreting the leading octal digit (combined use)

The three special bits can be combined. The leading octal digit is sum of:
- `4` = SUID
- `2` = SGID
- `1` = Sticky

Examples:
- `4755` → SUID + 755
- `2755` → SGID + 755
- `1777` → Sticky + 777
- `6755` → SUID + SGID + 755 (rare but possible)

Set by numeric `chmod`:
```bash
sudo chmod 2755 /shared     # sets SGID + 755
sudo chmod 4755 /usr/bin/tool  # sets SUID + rwxr-xr-x
sudo chmod 1777 /tmp
```

---

## Checking, locating and auditing special bits

- List a folder to visually check `s`/`t`:
```bash
ls -l /usr/bin/passwd
# -rwsr-xr-x 1 root root ...
```

- Search whole system:
```bash
# SUID
find / -perm /4000 -type f -ls 2>/dev/null

# SGID
find / -perm /2000 -type f -ls 2>/dev/null

# Sticky (directories)
find / -perm /1000 -type d -ls 2>/dev/null
```

- Show numeric mode:
```bash
stat -c "%n %a %A" /usr/bin/passwd
# /usr/bin/passwd 4755 -rwsr-xr-x
```

---

## Security considerations & best practices

- **SUID** and **SGID** on executables are powerful — they can be exploited if the program is vulnerable. Keep the list small and prefer well-maintained, auditable binaries.
- **Audit** SUID/SGID files regularly (use the `find` commands above).
- **Avoid SUID on scripts**; interpreted scripts are often ignored by kernels for SUID, and SUID scripts are hard to secure.
- For shared directories (SGID), prefer combining SGID with:
  - group-writable bits, and
  - a collaborative `umask` (e.g., `002`), or
  - POSIX ACLs to give fine-grained control.
- Use **sticky bit** for any world-writable directory (`/tmp`, upload drop folders) to prevent accidental or malicious deletion by other users.
- When restoring files from backups, ensure you preserve or reset special bits as appropriate (use `rsync -a` or `tar --preserve-permissions` and run as root where necessary).

---

## Quick command cheat-sheet

```bash
# SUID
sudo chmod u+s /path/to/program        # set SUID (symbolic)
sudo chmod 4755 /path/to/program       # set SUID (numeric)
sudo chmod u-s /path/to/program        # remove SUID
find / -perm /4000 -type f -ls         # list SUID files

# SGID
sudo chmod g+s /path/to/program        # set SGID on executable
sudo chmod 2755 /shared                # set SGID on directory + rwx perms
sudo chmod g-s /path/to/program        # remove SGID
find / -perm /2000 -type f -ls         # list SGID files/dirs

# Sticky bit
sudo chmod +t /some/dir                # set sticky
sudo chmod 1777 /tmp                   # common sticky + world writable
sudo chmod -t /some/dir                # remove sticky
find / -perm /1000 -type d -ls         # list directories with sticky bit
```

---

## Final recap

- **SUID (4)**: executable runs with the owner's UID (e.g., `passwd` → root privileges while running).  
- **SGID (2)**: directory: new files inherit directory group (shared workspace); executable: runs with file's group.  
- **Sticky (1)**: directory: only owner (or dir owner or root) can delete a file in that directory (used by `/tmp`).

These three bits extend the standard owner/group/other model and are essential for building collaborative file areas and controlled privilege escalation — but they must be used carefully for security.

---

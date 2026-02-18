# User Management in Linux

## Understanding Users in Linux

In Linux, **every entity that logs in or runs a process** is a **user**.  
Each user has:
- A **username**
- A unique **UID (User ID)**
- A **primary group (GID)**
- Optional **supplementary groups**
- A **home directory**
- A **login shell**

Linux stores user information mainly in:
- `/etc/passwd` → basic user info (username, UID, GID, shell, home)
- `/etc/shadow` → encrypted passwords and password aging info
- `/etc/group` → group memberships

---

## 1. Creating a New User

There are two common ways to create a user:
1. Using **`useradd`** (low-level command)
2. Using **`adduser`** (user-friendly interactive script)

---

### Method 1: Using `useradd`

Basic syntax:
```bash
sudo useradd [options] username
```

Example:
```bash
sudo useradd -m -s /bin/bash alice
```

**Explanation:**
- `-m` → creates a home directory `/home/alice`
- `-s /bin/bash` → sets default login shell to Bash
- Without `-m`, user has no home directory
- Default shell may differ based on distro (e.g., `/bin/sh`)

Then set password:
```bash
sudo passwd alice
```

Verify creation:
```bash
cat /etc/passwd | grep alice
```

Output example:
```
alice:x:1001:1001::/home/alice:/bin/bash
```

---

### Method 2: Using `adduser`

`adduser` is a **more user-friendly command** available on most Debian/Ubuntu systems (it’s a Perl script that uses `useradd` internally).

Example:
```bash
sudo adduser alice
```

**What happens automatically:**
- Creates user account **and home directory**
- Automatically creates a **group with the same name** (`alice`)
- Prompts you to **set a password interactively**
- Prompts for optional details (Full Name, Room Number, etc.)

You don’t need to use `-m` or `passwd` separately — `adduser` handles it all.

---

### Key Differences Between `useradd` and `adduser`

| Feature | `useradd` | `adduser` |
|----------|------------|-----------|
| Type | Low-level binary | High-level interactive script |
| Home directory | Must specify with `-m` | Automatically created |
| Password | Must run `passwd` manually | Prompts for password |
| Group creation | Must specify or use `-U` | Automatically creates same-named group |
| Interactivity | Non-interactive | Fully interactive |
| Recommended for | Scripts / Automation | Manual user creation |

---

## 2. Setting and Managing Passwords

Set or reset a user’s password:
```bash
sudo passwd alice
```

Lock or unlock password:
```bash
sudo passwd -l alice   # lock
sudo passwd -u alice   # unlock
```

---

## 3. Switching Between Users

Once a user is created, you can switch between users using the `su` (substitute user) command.

### Switch to another user:
```bash
su - alice
```
- The `-` (login shell) ensures you load the user’s environment (like logging in fresh).
- If you’re **root**, you **won’t be asked for a password**.
- If you’re switching from one **regular user** to another, it **will ask for the target user’s password**.

### Return to the previous user:
```bash
exit
```
This exits the current shell and returns to the previous user (e.g., root).

---

## 4. Modifying Existing Users

Modify users with:
```bash
sudo usermod [options] username
```

### Common options:

| Option | Description | Example |
|--------|--------------|----------|
| `-aG` | Add user to supplementary groups | `sudo usermod -aG devs,developers alice` |
| `-G`  | Replace user’s existing groups | `sudo usermod -G devops alice` |
| `-s`  | Change login shell | `sudo usermod -s /bin/zsh alice` |
| `-d`  | Change home directory | `sudo usermod -d /data/alice alice` |
| `-l`  | Change username | `sudo usermod -l alicia alice` |
| `-L`  | Lock user account | `sudo usermod -L alice` |
| `-U`  | Unlock user account | `sudo usermod -U alice` |

> - **Tip:**  
> - `-aG` adds groups without removing existing ones.  
> - Using only `-G` replaces all supplementary groups.

---

## 5. Checking User and Group Information

### Verify user creation
```bash
cat /etc/passwd | grep alice
```

### Check which groups a user belongs to
```bash
groups alice
# or
id alice
# or
cat /etc/group | grep alice
```

Example output:
```
alice : alice devs developers
```

---

## 6. Managing Groups

Groups define permissions and shared access.

### Add user to groups
```bash
sudo usermod -aG developers,devops alice
```

### Replace all supplementary groups
```bash
sudo usermod -G admins alice
```

### Change primary group
```bash
sudo usermod -g devs alice
```

---

## 7. Changing User’s Login Shell

View available shells:
```bash
cat /etc/shells
```

Change shell:
```bash
sudo usermod -s /bin/zsh alice
```

Verify:
```bash
grep alice /etc/passwd
# alice:x:1001:1001::/home/alice:/bin/zsh
```

---

## 8. Managing Home Directories

By default, when you use `adduser` or `useradd -m`, a home directory is created.

Set a custom home directory:
```bash
sudo useradd -m -d /data/alice alice
```

Move an existing user’s home:
```bash
sudo usermod -d /data/alice -m alice
```

---

## 9. Deleting Users

Delete a user (keep home):
```bash
sudo userdel alice
```

Delete a user and home directory:
```bash
sudo userdel -r alice
```

Remove a user from a group:
```bash
sudo gpasswd -d alice devs
```

---

## 10. System Files Involved

| File | Purpose |
|------|----------|
| `/etc/passwd` | Stores user info (username, UID, GID, home, shell) |
| `/etc/shadow` | Stores encrypted passwords |
| `/etc/group` | Group memberships |
| `/etc/login.defs` | Default user creation settings |
| `/etc/skel/` | Files copied to new user home directory |

---

## 11. Example Workflow — Adding a Developer Account

1. **Create user**
   ```bash
   sudo adduser alice
   ```

2. **Add to project groups**
   ```bash
   sudo usermod -aG devs,developers,devops alice
   ```

3. **Change shell**
   ```bash
   sudo usermod -s /bin/zsh alice
   ```

4. **Switch to user**
   ```bash
   su - alice
   ```

5. **Return to root**
   ```bash
   exit
   ```

6. **Verify**
   ```bash
   id alice
   cat /etc/passwd | grep alice
   groups alice
   ```

7. **Remove user (when offboarding)**
   ```bash
   sudo userdel -r alice
   ```

---

## 12. Best Practices

- Use `adduser` for simplicity and interactivity.  
- Use `useradd` in scripts or automated setups.  
- Always set passwords immediately after user creation.  
- Use `sudo usermod -aG` (never forget `-a`) when adding to groups.  
- Verify users with `id`, `groups`, or `/etc/passwd`.  
- Lock unused accounts instead of deleting them immediately.  
- Regularly audit `/etc/passwd` and `/etc/group`.

---

## 13. Quick Command Reference

```bash
# Create user with adduser
sudo adduser alice

# Create user with useradd
sudo useradd -m -s /bin/bash alice
sudo passwd alice

# Switch to user
su - alice

# Return to root
exit

# Add user to groups
sudo usermod -aG devs,developers alice

# Change shell
sudo usermod -s /bin/zsh alice

# Verify user
cat /etc/passwd | grep alice
groups alice
id alice

# Delete user
sudo userdel -r alice
```

---

## Final Recap

| Task | Command Example |
|------|------------------|
| Create user (interactive) | `adduser alice` |
| Create user (manual) | `useradd -m -s /bin/bash alice` |
| Set password | `passwd alice` |
| Switch to user | `su - alice` |
| Return to root | `exit` |
| Add to groups | `usermod -aG devs,devops alice` |
| Change shell | `usermod -s /bin/zsh alice` |
| Verify creation | `cat /etc/passwd | grep alice` |
| Check groups | `groups alice` / `id alice` |
| Delete user | `userdel -r alice` |

---

Linux user management is a **core skill** — understanding how users, groups, shells, and permissions interact is the foundation of secure and organized system administration.

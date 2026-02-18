# Group Management in Linux

## 1. What is a Group in Linux?

A **group** in Linux is a **collection of users**.  
Groups simplify permission management — instead of giving access to each user individually, you can assign permissions to a group, and all its members automatically inherit those permissions.

Each user in Linux belongs to:
- **One Primary Group** → defined when the user is created.
- **Zero or more Supplementary Groups** → added later for shared access.

---

## 2. Where Group Information is Stored

Group data is stored in:
- `/etc/group` → group name, GID, and members
- `/etc/gshadow` → secure group passwords and administrative info

You can inspect all groups on your system with:
```bash
cat /etc/group
```

Example output:
```
root:x:0:
devs:x:1001:alice,bob
```

---

## 3. Types of Groups

| Type | Description | Example |
|------|--------------|----------|
| **Primary Group** | Automatically assigned when a user is created. All files created by the user belong to this group by default. | The group with the same name as the user (e.g., `alice:alice`). |
| **Supplementary Group** | Additional groups a user can join for collaborative access. | `devs`, `developers`, `admin` |

---

## 4. Group Management Commands

### Creating a Group
```bash
sudo groupadd developers
```

This creates a new group named `developers`.

To verify:
```bash
cat /etc/group | grep developers
```
Output:
```
developers:x:1002:
```

> Each group is identified by a unique **GID (Group ID)**.

---

### Deleting a Group
```bash
sudo groupdel developers
```

This removes the group from `/etc/group`.

> Deleting a group does **not** delete the users in it.  
> However, any files that previously belonged to this group will still show the old GID until ownership is changed.

---

### Renaming (Modifying) a Group
```bash
sudo groupmod -n devs developers
```

This renames the group `developers` → `devs`.

Verify:
```bash
cat /etc/group | grep devs
```

---

## 5. Adding Users to Groups

To make collaboration easier, you can add users to groups.

### Add a user to supplementary groups
```bash
sudo usermod -aG developers alice
```

> Don’t forget the `-a` flag — it **appends** the user to the group(s).  
> Without it, existing group memberships are replaced.

Add multiple users:
```bash
sudo gpasswd -a bob developers
```

Add multiple groups:
```bash
sudo usermod -aG devs,devops alice
```

---

## 6. Viewing Group Memberships

Check all groups a user belongs to:
```bash
groups alice
```
or
```bash
id alice
```

Check who is in a specific group:
```bash
getent group developers
```

Example output:
```
developers:x:1002:alice,bob
```

---

## 7. Removing a User from a Group

Remove a user:
```bash
sudo gpasswd -d alice developers
```

This removes `alice` from the `developers` group.

---

## 8. Changing Group Ownership of Files

You can change which group owns a file or directory using `chown` or `chgrp`.

### Using `chgrp`
```bash
sudo chgrp developers project.txt
```

### Using `chown` (user:group format)
```bash
sudo chown :developers project.txt
```

This changes only the group ownership (not the file owner).

---

## 9. Assigning a Primary Group to a User

When a user is created, a **primary group** is assigned automatically (same name as the user).

You can change a user’s primary group using:
```bash
sudo usermod -g devs alice
```

Now any new file `alice` creates will have `devs` as its group.

---

## 10. Viewing Group Information

### To list all groups:
```bash
cut -d: -f1 /etc/group
```

### To see details of a specific group:
```bash
getent group devs
```

Output:
```
devs:x:1001:alice,bob
```

---

## 11. Real-World Example — Team Collaboration

Let’s say you have two developers (`alice`, `bob`) who need to collaborate in a shared project folder `/shared`.

### Step 1: Create a group
```bash
sudo groupadd devs
```

### Step 2: Assign group ownership to folder
```bash
sudo chgrp devs /shared
```

### Step 3: Add users to the group
```bash
sudo usermod -aG devs alice
sudo usermod -aG devs bob
```

### Step 4: Verify
```bash
ls -ld /shared
# drwxr-xr-x  2 root devs 4096 Oct 18 14:00 /shared
```

Now both users can access `/shared` according to group permissions.

---

## 12. Best Practices

- Always use meaningful group names (`devs`, `admins`, `designers`).
- Use **groups** instead of changing file permissions for each user.
- Keep a consistent naming pattern across teams.
- Regularly check `/etc/group` for old or unused groups.
- When adding users, **always use `-aG`** to avoid overwriting existing groups.

---

## 13. Quick Command Reference

| Task | Command |
|------|----------|
| Create a group | `sudo groupadd developers` |
| Delete a group | `sudo groupdel developers` |
| Rename a group | `sudo groupmod -n devs developers` |
| Add user to group | `sudo usermod -aG developers alice` |
| Remove user from group | `sudo gpasswd -d alice developers` |
| Check user’s groups | `groups alice` or `id alice` |
| View members of group | `getent group developers` |
| Change group ownership of file | `sudo chgrp developers file.txt` |
| Change user’s primary group | `sudo usermod -g devs alice` |

---

## Final Recap

| Concept | Description | Example |
|----------|--------------|----------|
| **Group** | Collection of users with shared permissions | `devs`, `admins` |
| **Primary Group** | Default group assigned to user | `alice:alice` |
| **Supplementary Groups** | Extra groups for collaboration | `devs`, `docker` |
| **Create Group** | `sudo groupadd developers` |
| **Rename Group** | `sudo groupmod -n devs developers` |
| **Delete Group** | `sudo groupdel developers` |
| **Add User to Group** | `sudo usermod -aG devs alice` |
| **Remove User from Group** | `sudo gpasswd -d alice devs` |

---

Group management is an essential part of **Linux access control** — it ensures smooth collaboration, cleaner permission management, and secure file sharing between users.

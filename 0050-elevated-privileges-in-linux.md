## Introduction to Elevated Privileges in Linux

### 1. Introduction — Understanding Elevated Privileges in Linux

Linux is designed as a **multi-user operating system** with strict privilege separation.

Not every user is allowed to:

✔ Modify system files  
✔ Install software  
✔ Manage services  
✔ Access protected resources  

Elevated privileges exist to safely perform administrative tasks.

Understanding this correctly is critical for system stability and security.

---

## PART 1 — Why Privileges Exist

---

### 2. The Core Security Model

Linux separates:

✔ Regular users → Limited permissions  
✔ Root user → Full system control  

Root is the most powerful account.

Mistakes as root can break the entire system.

---

### 3. Principle of Least Privilege

Best practice:

> Operate as normal user → Elevate only when required

---

## PART 2 — Switching Users with `su`

---

### 4. What is `su`?

`su` stands for:

✔ **Substitute User**  
✔ **Switch User**

Allows changing identity inside terminal.

---

### 5. Basic Syntax

	su - <username>

Example:

	su - student1

---

### 6. Why Use the Dash (`-`)?

This is extremely important.

✔ `su student1` → Switch user only  
✔ `su - student1` → Full login shell  

With dash:

✔ Loads user environment  
✔ Applies user’s profile  
✔ Correct PATH variables  

Always preferred.

---

### 7. Authentication Behavior

When using `su`:

✔ Requires target user’s password

Not your password.

---

### 8. Common Use Cases

✔ Testing another user’s environment  
✔ Administrative workflows  
✔ Multi-user debugging  

---

## PART 3 — Running Commands as Root with `sudo`

---

### 9. What is `sudo`?

`sudo` stands for:

✔ **Superuser Do**

Executes a command with elevated privileges.

---

### 10. Basic Syntax

	sudo <command>

Example:

	sudo apt update

---

### 11. Key Security Advantage

Unlike `su`:

✔ Uses YOUR password  
✔ Logs command usage  
✔ Granular control  

Much safer.

---

### 12. Why `sudo` is Preferred in Modern Linux

✔ Better auditing  
✔ Reduced root exposure  
✔ Fine-grained permissions  

---

## PART 4 — Starting a Root Shell (`sudo -i`)

---

### 13. What Does `sudo -i` Do?

Command:

	sudo -i

Meaning:

✔ Start interactive login shell as root  
✔ Load root environment  

You now operate fully as root.

---

### 14. How This Differs from Single Commands

Instead of:

	sudo command1
	sudo command2
	sudo command3

You elevate once.

---

### 15. When to Use `sudo -i`

✔ Multiple administrative commands  
✔ System maintenance  
✔ Debugging sessions  

---

### 16. How to Exit Root Shell

	exit

Returns to normal user.

---

## PART 5 — Running Root Shell via `sudo su`

---

### 17. What is `sudo su`?

Command:

	sudo su

Meaning:

✔ Run `su` command using sudo  
✔ Switch to root user  

---

### 18. Important Difference vs `sudo -i`

**sudo -i**

✔ Clean root login shell  
✔ Proper environment loading  

**sudo su**

✔ Switch user behavior  
✔ Slightly different environment handling  

---

### 19. Best Practice Recommendation

Prefer:

✔ `sudo -i`

Avoid unnecessary chaining unless needed.

---

## PART 6 — Understanding the Differences Clearly

---

### 20. `su - user`

✔ Requires target password  
✔ Switch identity  

---

### 21. `sudo command`

✔ Temporary privilege escalation  
✔ One command only  

---

### 22. `sudo -i`

✔ Full root shell  
✔ Proper root environment  

---

### 23. `sudo su`

✔ Root shell via chaining  
✔ Slightly less clean approach  

---

## PART 7 — Security Best Practices

---

### 24. Avoid Direct Root Login

✔ Use sudo instead  
✔ Better accountability  

---

### 25. Elevate Only When Necessary

✔ Minimizes risk  
✔ Prevents accidental damage  

---

### 26. Never Stay Root Longer Than Needed

✔ Reduces mistake probability  

---

### 27. Understand the Risk of Root Privileges

Root can:

✔ Delete entire filesystem  
✔ Break boot process  
✔ Corrupt system configuration  

No safety nets.

---

## PART 8 — Mental Model of Privilege Escalation

---

### 28. Think of Root as “Unsafe Mode”

✔ Maximum power  
✔ Maximum responsibility  

---

### 29. Proper Workflow Mindset

✔ Normal user → Default  
✔ Sudo → Tool  
✔ Root → Exceptional state  

---

### 30. Final Thoughts

Privilege management is one of the most fundamental Linux skills.

Misunderstanding it leads to:

✘ Security vulnerabilities  
✘ System instability  
✘ Operational mistakes  

Correct usage leads to:

✔ Secure administration  
✔ Stable systems  
✔ Professional workflows

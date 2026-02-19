## Understanding `who` command

### 1. Introduction — Understanding the `who` Command

In multi-user operating systems like Linux, it is often important to know:

✔ Who is logged in  
✔ From where they are connected  
✔ When they logged in  

The `who` command provides this visibility.

It is a **user session inspection tool**.

---

### 2. What Does `who` Do?

The `who` command displays:

> Information about users currently logged into the system.

Basic usage:

	who

---

### 3. Example Output

Typical output:

	user1   pts/0   2026-02-19 10:15 (192.168.1.50)
	user2   pts/1   2026-02-19 10:20 (10.0.0.12)

Each column has meaning.

---

## PART 1 — Understanding the Output Fields

---

### 4. Username

First column:

	user1

Represents:

✔ Logged-in user account

---

### 5. Terminal (TTY / PTS)

Second column:

	pts/0

Indicates:

✔ Session terminal

Two common types:

**tty** → Local login (physical machine)  
**pts** → Remote login (SSH sessions)

---

### 6. Why `pts` is Common Today

Most modern logins are:

✔ SSH sessions  
✔ Terminal emulators  
✔ Remote connections  

---

### 7. Login Date & Time

Example:

	2026-02-19 10:15

Represents:

✔ When session started

Useful for:

✔ Auditing  
✔ Debugging  
✔ Security monitoring  

---

### 8. Remote Host / IP Address

Example:

	(192.168.1.50)

Indicates:

✔ Origin of login

Critical for:

✔ Security analysis  
✔ Detecting suspicious access  

---

---

## PART 2 — Practical Uses of `who`

---

### 9. Check Active Users

	who

Quickly answers:

✔ Is anyone else logged in?

---

### 10. Detect Remote Logins

Look for:

✔ `pts/*` sessions  
✔ IP addresses  

---

### 11. Security Monitoring

Helps identify:

✔ Unexpected users  
✔ Unknown IP addresses  
✔ Suspicious sessions  

---

---

## PART 3 — Useful Variations of `who`

---

### 12. Show System Boot Time

	who -b

Output example:

	system boot  2026-02-19 09:55

Useful for:

✔ Uptime checks  
✔ Troubleshooting  

---

### 13. Show Current User Only

	who -m

Displays:

✔ Your session details

Equivalent concept:

✔ "Who am I?"

---

### 14. Show All Details

	who -a

Provides:

✔ Users  
✔ Dead processes  
✔ Login processes  
✔ Runlevel  

More diagnostic-heavy.

---

---

## PART 4 — Related Commands (Important Distinction)

---

### 15. `whoami`

Command:

	whoami

Meaning:

✔ Prints current username only

---

### 16. `w` Command

Command:

	w

Provides richer info:

✔ Logged-in users  
✔ What they are doing  
✔ CPU usage  
✔ Idle time  

Think of:

✔ `who` → Who is logged in  
✔ `w` → Who + Activity  

---

### 17. `users` Command

	users

Displays:

✔ Simple list of usernames

Minimal output.

---

---

## PART 5 — Mental Model of `who`

---

### 18. What `who` is Really Showing

Internally, `who` reads:

✔ Session records  
✔ Login database (`utmp`)

It reflects:

✔ Active login sessions

---

### 19. Why This Matters

Linux is designed as:

✔ Multi-user system  
✔ Concurrent sessions  
✔ Shared resources  

Knowing active users is fundamental.

---

### 20. Final Thoughts

The `who` command is a simple but powerful tool for:

✔ System awareness  
✔ Session inspection  
✔ Security visibility  
✔ Troubleshooting  

Small command.

Extremely practical in real systems.

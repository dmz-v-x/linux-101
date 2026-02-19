## Troubleshooting Common SSH Issues

### 1. Introduction

SSH is extremely reliable, but when something goes wrong, the errors can feel cryptic.

Effective troubleshooting is not about memorizing fixes.

It is about understanding:

✔ Where failure occurs  
✔ What SSH is trying to do  
✔ Which component is responsible  

This guide builds a systematic debugging mindset.

---

### 2. First Principle of SSH Troubleshooting

Always ask:

> "At which stage is the connection failing?"

SSH connection phases:

1. Network connectivity
2. Server reachability
3. Cryptographic handshake
4. Authentication
5. Session stability

Every error belongs to one of these layers.

---

## PART 1 — "Permission Denied" Error

---

### 3. What This Error Actually Means

Example:

	Permission denied (publickey,password).

Meaning:

✔ Server reachable  
✔ SSH running  
✘ Authentication failed  

This is an identity problem, not a network problem.

---

### 4. Most Common Causes

✔ Wrong username  
✔ Wrong private key  
✔ Key not present in authorized_keys  
✔ Incorrect permissions  

---

### 5. Verify Correct Key Usage

Explicitly specify key:

	ssh -i ~/.ssh/my_private_key user@hostname

Important when:

- Multiple keys exist
- Non-default naming used

---

### 6. Fix Local Key Permissions

SSH rejects insecure keys.

	chmod 600 ~/.ssh/my_private_key

---

### 7. Fix Server Permissions

On server:

	chmod 700 ~/.ssh
	chmod 600 ~/.ssh/authorized_keys

SSH is extremely strict here.

---

### 8. Debug with Verbose Output

	ssh -v user@hostname

Look for:

✔ Which keys are attempted  
✔ Why keys are rejected  

---

## PART 2 — "Connection Refused"

---

### 9. What This Error Means

Example:

	Connection refused

Meaning:

✔ Server reachable  
✘ SSH service not accepting connections  

---

### 10. Most Likely Causes

✔ SSH service not running  
✔ Firewall blocking port  
✔ Wrong port  

---

### 11. Check SSH Service on Server

	sudo systemctl status ssh
	sudo systemctl restart ssh

---

### 12. Verify SSH Listening Port

	ss -tnlp | grep ssh

Confirms:

✔ Port number  
✔ Service binding  

---

### 13. Check Firewall Rules

Example (UFW):

	sudo ufw status
	sudo ufw allow ssh

---

### 14. Wrong Port Scenario

If SSH uses port 2222:

	ssh -p 2222 user@hostname

---

## PART 3 — "Connection Timed Out"

---

### 15. What This Error Means

Meaning:

✘ No response from server

Unlike "refused":

✔ Packets not reaching service  

---

### 16. Common Causes

✔ Wrong IP address  
✔ Firewall dropping packets  
✔ Network routing issues  

---

### 17. Test Basic Connectivity

	ping hostname_or_ip

If ping fails:

✔ Network-level issue  

---

### 18. Trace Network Path

	traceroute hostname_or_ip

Helps locate:

✔ Where packets stop  

---

### 19. Firewall Diagnosis

Temporarily disable (testing only):

	sudo ufw disable

If SSH works afterward:

✔ Firewall misconfiguration  

---

## PART 4 — "Host Key Verification Failed"

---

### 20. What This Error Means

SSH detected:

✘ Server identity mismatch

This is a **security protection**, not a bug.

---

### 21. Why It Happens

✔ Server reinstalled  
✔ Host keys regenerated  
✔ Possible MITM attack  

---

### 22. Remove Old Host Key

	ssh-keygen -R hostname_or_ip

Removes stale entry.

---

### 23. Reconnect Carefully

	ssh user@hostname

Always verify fingerprint if system is sensitive.

---

### 24. Inspect Server Key Manually

	ssh-keyscan hostname_or_ip

---

## PART 5 — "No Route to Host"

---

### 25. What This Error Means

Meaning:

✘ Network cannot reach server

Packets fail before SSH even begins.

---

### 26. Likely Causes

✔ Server offline  
✔ Incorrect network configuration  
✔ Routing issues  
✔ Cloud firewall restrictions  

---

### 27. Check Server Network Interfaces

On server:

	ip addr

Verify:

✔ Valid IP assigned  

---

### 28. Check Routing Table

	ip route

Ensures proper gateway exists.

---

### 29. Infrastructure-Level Causes

Very common in:

✔ Cloud environments  
✔ Misconfigured security groups  
✔ VPN setups  

---

## PART 6 — "Too Many Authentication Failures"

---

### 30. What This Error Means

SSH tried too many identities.

Server terminated connection defensively.

---

### 31. Why It Happens

✔ Many keys loaded in agent  
✔ SSH attempts wrong keys repeatedly  

---

### 32. Force Correct Key Usage

	ssh -o IdentitiesOnly=yes -i ~/.ssh/my_private_key user@hostname

---

### 33. Clear SSH Agent

	ssh-add -D

Removes cached identities.

---

## PART 7 — Dropped / Hanging SSH Sessions

---

### 34. Symptoms

✔ Random disconnects  
✔ Frozen terminals  
✔ Idle session drops  

---

### 35. Common Causes

✔ NAT timeout  
✔ Firewall idle limits  
✔ Unstable network  

---

### 36. Configure KeepAlive (Client-Side)

In:

	~/.ssh/config

Add:

	ServerAliveInterval 60
	ServerAliveCountMax 3

---

### 37. Why KeepAlive Works

✔ Prevents idle disconnects  
✔ Detects dead sessions  

---

### 38. Diagnose Network Stability

	ping hostname_or_ip
	mtr hostname_or_ip

Look for:

✔ Packet loss  
✔ Latency spikes  

---

## PART 8 — "Could Not Resolve Hostname"

---

### 39. What This Error Means

Meaning:

✘ DNS resolution failure

SSH cannot translate hostname → IP.

---

### 40. Common Causes

✔ Typo in hostname  
✔ DNS misconfiguration  
✔ Missing DNS entry  

---

### 41. Test DNS Resolution

	ping hostname
	dig hostname

---

### 42. Use IP Address Directly

	ssh user@ip_address

Bypasses DNS entirely.

---

## PART 9 — Universal Debugging Technique

---

### 43. Always Use Verbose Mode When Confused

	ssh -vvv user@hostname

This reveals:

✔ Handshake details  
✔ Key negotiation  
✔ Authentication logic  
✔ Failure reason  

---

### 44. Think in Layers (Professional Mindset)

Never treat SSH errors randomly.

Categorize:

✔ Network issue  
✔ Service issue  
✔ Identity issue  
✔ Trust issue  
✔ Stability issue  

SSH debugging becomes predictable once layered thinking is applied.

---

### 45. Final Thoughts

SSH problems are rarely mysterious.

They are usually:

✔ Misconfigurations  
✔ Permission issues  
✔ Wrong assumptions  

The key skill is not fixing errors.

It is **diagnosing correctly**.

Correct diagnosis → Fast resolution  
Incorrect diagnosis → Endless frustration

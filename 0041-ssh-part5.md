## Security Best Practices

### 1. Introduction

SSH is inherently secure by design, but **default security is not the same as hardened security**.

Most real-world SSH compromises occur not because SSH is weak, but because:

- Configurations are permissive
- Credentials are poorly managed
- Systems are insufficiently monitored

Security best practices transform SSH from “secure enough” into “defense-grade secure”.

This guide builds that mindset step by step.

---

## PART 1 — Authentication Hardening

---

### 2. Why Authentication is the Primary Attack Surface

Attackers almost always target:

✔ Login mechanisms  
✔ Credentials  
✔ Identity validation  

Strengthening authentication yields the highest security gains.

---

### 3. Use SSH Keys Instead of Passwords

Passwords are vulnerable to:

- Brute force attacks
- Credential stuffing
- Phishing
- Human weakness

SSH keys rely on cryptography, making them vastly stronger.

---

### 4. Disable Password Authentication (Server-Side)

Edit:

	/etc/ssh/sshd_config

Set:

	PasswordAuthentication no

Effect:

✔ Password logins blocked  
✔ Keys required  

---

### 5. Why This is a Major Security Upgrade

Without passwords:

✘ No brute force attacks  
✘ No password leaks  
✔ Cryptographic identity only  

---

### 6. Generate Strong SSH Keys

Modern recommendation:

	ssh-keygen -t ed25519 -C "your_email@example.com"

Alternative (RSA):

	ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

---

### 7. Key Type Selection Insight

**ed25519**

✔ Faster  
✔ Smaller  
✔ Stronger modern security  

**RSA 4096**

✔ Widely compatible  
✔ Strong when large enough  

---

## PART 2 — Root Login Protection

---

### 8. Why Root Login is Dangerous

Root is:

✔ Most privileged account  
✔ High-value attack target  

Allowing direct root login increases risk.

---

### 9. Disable Root Login

Edit:

	/etc/ssh/sshd_config

Set:

	PermitRootLogin no

---

### 10. Recommended Workflow

✔ Login as regular user  
✔ Escalate via sudo  

Security benefits:

✔ Auditability  
✔ Reduced attack surface  

---

## PART 3 — System Updates & Patch Management

---

### 11. Why Updates are Critical

Many breaches exploit:

✘ Known vulnerabilities  
✘ Already patched issues  

---

### 12. Keep System Updated

**Ubuntu / Debian**

	sudo apt update && sudo apt upgrade

**RHEL / CentOS**

	sudo yum update

---

### 13. Why This Matters for SSH

✔ Security patches  
✔ Cryptographic improvements  
✔ Bug fixes  

---

## PART 4 — Restricting SSH Access

---

### 14. Principle of Least Exposure

Default SSH:

✔ Accessible from anywhere  

Better:

✔ Accessible only when necessary  

---

### 15. Restrict by IP (Server-Side)

In:

	/etc/ssh/sshd_config

Example:

	AllowUsers johndoe@192.168.1.100

or

	AllowUsers johndoe@192.168.1.0/24

---

### 16. Security Benefits

✔ Blocks random internet scans  
✔ Reduces brute force noise  
✔ Limits attacker reach  

---

### 17. Even Better — Firewall Restrictions

Example (UFW):

	sudo ufw allow from 192.168.1.100 to any port 22

Stronger control layer.

---

## PART 5 — Two-Factor Authentication (2FA)

---

### 18. Why Keys Alone May Not Be Enough

If private key is stolen:

✔ Attacker gains access  

2FA adds another barrier.

---

### 19. 2FA Security Model

Requires:

✔ Something you have (key/device)  
✔ Something you know (code/passphrase)

---

### 20. Example — Google Authenticator Setup

Install:

	sudo apt install libpam-google-authenticator

Enable challenge response:

	ChallengeResponseAuthentication yes

---

### 21. User-Level Setup

Run:

	google-authenticator

Produces:

✔ Secret key  
✔ Recovery codes  

---

### 22. Security Impact

Even with stolen key:

✘ Login blocked without OTP  

---

## PART 6 — User Access Control

---

### 23. Limit SSH Access to Specific Users

In:

	/etc/ssh/sshd_config

Example:

	AllowUsers johndoe jane_doe

---

### 24. Deny Specific Users

	DenyUsers root

Useful for layered defense.

---

### 25. Why This Matters

✔ Prevents unnecessary accounts from remote access  
✔ Reduces insider risk  
✔ Simplifies monitoring  

---

## PART 7 — SSH Key Passphrases

---

### 26. Why Passphrases are Still Important

Without passphrase:

✔ Key file = immediate access  

With passphrase:

✔ Key file encrypted  

---

### 27. Add Passphrase to Existing Key

	ssh-keygen -p -f ~/.ssh/id_ed25519

---

### 28. Real Security Benefit

If laptop stolen:

✘ Key unusable without passphrase  

---

## PART 8 — Logging & Monitoring

---

### 29. Why Monitoring is Non-Negotiable

Security is not just prevention.

It is:

✔ Detection  
✔ Visibility  
✔ Incident response  

---

### 30. Enable Verbose Logging

Edit:

	/etc/ssh/sshd_config

Set:

	LogLevel VERBOSE

---

### 31. Monitoring SSH Logs

Common log file:

	/var/log/auth.log

Example:

	cat /var/log/auth.log | grep sshd

---

### 32. What Logs Reveal

✔ Failed login attempts  
✔ Key fingerprints  
✔ Suspicious activity  
✔ Brute force patterns  

---

## PART 9 — Port Forwarding Controls

---

### 33. Why Port Forwarding Can Be Risky

Port forwarding can:

✔ Expose internal services  
✔ Bypass firewall controls  

---

### 34. Disable If Not Needed

	AllowTcpForwarding no

---

### 35. Enable Only When Required

	AllowTcpForwarding yes

Security principle:

✔ Minimize features → Minimize risk

---

## PART 10 — Protocol Hardening

---

### 36. Disable SSH Protocol 1

Protocol 1 is:

✘ Obsolete  
✘ Cryptographically weak  

---

### 37. Enforce Protocol 2

	Protocol 2

Modern systems already default to this, but explicit is safer.

---

## PART 11 — Advanced Defensive Measures

---

### 38. Change Default SSH Port (Optional)

Security through obscurity is not primary defense, but reduces noise.

Example:

	Port 2222

Benefit:

✔ Reduces automated scanning attacks

---

### 39. Use Fail2Ban (Highly Recommended)

Automatically blocks:

✔ Brute force attackers  
✔ Repeated failed attempts  

---

### 40. Disable Unused Authentication Methods

Example:

	ChallengeResponseAuthentication no
	KerberosAuthentication no

Reduces attack surface.

---

### 41. Final Thoughts

SSH security is about layered defense:

✔ Strong authentication  
✔ Reduced exposure  
✔ Controlled privileges  
✔ Proper monitoring  
✔ Minimal attack surface  

There is no single “magic setting”.

Security emerges from **combined discipline**.

SSH is secure by design.

Hardened SSH is secure in reality.

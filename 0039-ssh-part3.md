## Managing SSH Keys and Configurations

### 1. Introduction

Managing SSH keys and configurations is where SSH usage transitions from “basic connectivity” to “professional-grade workflow”.

Poor key management leads to:

- Authentication failures
- Security risks
- Operational confusion

Proper management results in:

- Seamless logins
- Strong security posture
- Clean multi-server workflows

This guide builds your understanding step by step.

---

## PART 1 — Managing SSH Keys

---

### 2. Understanding the `.ssh` Directory

SSH stores identity-related data inside:

	~/.ssh/

Think of this directory as:

> Your SSH identity vault

It typically contains:

- Private keys
- Public keys
- Known hosts
- Configuration files

---

### 3. Viewing Existing SSH Keys

To inspect your SSH assets:

	ls ~/.ssh/

Common files:

	id_rsa
	id_rsa.pub
	id_ed25519
	id_ed25519.pub
	known_hosts
	config

Key principle:

> Never modify files here casually

---

### 4. Identifying Key Pairs

SSH keys always come in pairs:

**Private Key**

	id_rsa
	id_ed25519

**Public Key**

	id_rsa.pub
	id_ed25519.pub

Simple rule:

✔ `.pub` → shareable  
✘ No extension → secret

---

### 5. Generating a New SSH Key Pair

If a key does not exist or a new identity is needed:

	ssh-keygen -t ed25519 -C "your_email@example.com"

Alternative (RSA):

	ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

Explanation:

- `-t` → key algorithm
- `-b` → key size (RSA only)
- `-C` → label/comment

---

### 6. Naming Keys (Critical for Multi-Key Workflows)

Instead of default naming:

Enter custom names:

	~/.ssh/work_key
	~/.ssh/personal_key

Why this matters:

Without naming discipline:

- Keys become confusing
- Debugging becomes painful

---

### 7. Adding Keys to SSH Agent

SSH agent stores decrypted keys in memory.

Start agent:

	eval "$(ssh-agent -s)"

Add key:

	ssh-add ~/.ssh/work_key

Benefits:

- Avoid repeated passphrase prompts
- Cleaner authentication flow

---

### 8. Managing Multiple Keys

Add multiple identities:

	ssh-add ~/.ssh/work_key
	ssh-add ~/.ssh/personal_key

Check loaded keys:

	ssh-add -l

---

### 9. Copying Public Key to Remote Server

Automatic method:

	ssh-copy-id user@hostname

This safely appends your key to:

	~/.ssh/authorized_keys (on server)

Manual alternative:

	cat ~/.ssh/work_key.pub | ssh user@host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

---

### 10. Removing Keys from Agent

Clear specific key:

	ssh-add -d ~/.ssh/work_key

Clear all keys:

	ssh-add -D

Useful when:

- Rotating identities
- Debugging authentication issues

---

## PART 2 — Managing SSH Configurations

---

### 11. Why the SSH Config File is a Game-Changer

File:

	~/.ssh/config

Purpose:

> Define connection logic once, reuse forever

Without config:

	ssh user@hostname -i key -p port

With config:

	ssh alias

---

### 12. Creating the Config File

Open editor:

	nano ~/.ssh/config

If missing, SSH will create it.

---

### 13. Basic Config Structure

Example:

	Host myserver
	    HostName 192.168.1.10
	    User johndoe
	    IdentityFile ~/.ssh/work_key
	    Port 22

Each line defines connection behavior.

---

### 14. Understanding Config Fields

**Host**  
Alias (nickname)

**HostName**  
Real IP/domain

**User**  
Login username

**IdentityFile**  
Which private key to use

**Port**  
Custom SSH port

---

### 15. Multi-Server Configuration Example

	Host production
	    HostName prod.example.com
	    User deploy
	    IdentityFile ~/.ssh/prod_key

	Host staging
	    HostName staging.example.com
	    User tester
	    IdentityFile ~/.ssh/stage_key
	    Port 2222

Now:

	ssh production
	ssh staging

---

### 16. Why IdentityFile is Extremely Important

If omitted:

- SSH guesses keys
- Authentication delays occur
- Failures become unpredictable

Explicit is always better.

---

### 17. Advanced Config Options

Useful professional settings:

**Connection Timeout**

	ConnectTimeout 10

**Keep Alive**

	ServerAliveInterval 60

**Disable Password Auth**

	PasswordAuthentication no

**Force Key Usage**

	IdentitiesOnly yes

---

### 18. Example of Hardened Config Entry

	Host secure-server
	    HostName 192.168.1.50
	    User admin
	    IdentityFile ~/.ssh/admin_key
	    IdentitiesOnly yes
	    ConnectTimeout 5

---

## PART 3 — Securing SSH Keys

---

### 19. Why Private Key Security is Non-Negotiable

Your private key is:

> Equivalent to your password — but more powerful

If stolen:

- Full server access granted

---

### 20. Using Passphrases (Strongly Recommended)

During key generation:

Enter passphrase.

Protection provided:

✔ Key file encryption  
✔ Defense against file theft

---

### 21. Adding Passphrase to Existing Key

	ssh-keygen -p -f ~/.ssh/work_key

Allows:

- Updating passphrase
- Adding one if missing

---

### 22. Correct File Permissions (Common Failure Point)

SSH is extremely strict.

Fix permissions:

	chmod 700 ~/.ssh
	chmod 600 ~/.ssh/work_key
	chmod 644 ~/.ssh/work_key.pub

If permissions are too open:

✘ SSH refuses key usage

---

### 23. Why Permissions Matter

Prevents:

- Other users reading keys
- Credential leakage
- Security compromise

---

### 24. Key Expiration (Advanced Security Strategy)

Optional but powerful.

Example:

	ssh-keygen -t ed25519 -C "temp-access" -V +52w

Meaning:

- Key valid for 52 weeks

Useful for:

- Contractors
- Temporary access
- Compliance environments

---

## PART 4 — Professional Key Management Strategies

---

### 25. Separate Keys by Purpose

Best practice:

✔ Work key  
✔ Personal key  
✔ Production key  
✔ Automation key

Never reuse blindly.

---

### 26. Avoid Key Sprawl

Too many unmanaged keys cause:

- Debugging nightmares
- Security blind spots
- Operational errors

Maintain discipline.

---

### 27. Backing Up Keys Securely

Golden rules:

✔ Encrypted backup only  
✔ Never plain-text cloud storage  
✔ Prefer password managers / encrypted vaults

---

### 28. Key Rotation Strategy

For sensitive systems:

- Periodically regenerate keys
- Remove stale keys from servers
- Audit authorized_keys

---

## PART 5 — Troubleshooting Key Issues

---

### 29. Permission Denied (Publickey)

Common causes:

- Wrong IdentityFile
- Key not in authorized_keys
- Incorrect permissions
- Key not loaded in agent

---

### 30. Debugging SSH Authentication

Verbose mode:

	ssh -v user@host

Shows:

- Key negotiation
- Authentication attempts
- Failure reasons

---

### 31. Too Many Authentication Failures

Cause:

- SSH trying multiple keys

Fix:

	IdentitiesOnly yes

---

### 32. Final Thoughts

SSH mastery is not about memorizing commands.

It is about:

✔ Identity management  
✔ Trust management  
✔ Secure workflows  
✔ Predictable connections  

Well-managed SSH environments feel invisible.

Poorly managed ones feel chaotic.

The difference is entirely in key and configuration discipline.

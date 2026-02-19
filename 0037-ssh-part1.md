## Introduction to SSH

### 1. Introduction to SSH

SSH (Secure Shell) is one of the most essential tools in modern computing. Whether you're managing cloud servers, deploying applications, or securely transferring files, SSH is the backbone of remote system administration.

At its core, SSH allows you to:

- Connect to remote machines securely
- Execute commands remotely
- Transfer files safely
- Tunnel other network traffic

This guide walks step by step from absolute basics to more advanced concepts.

---

### 2. What is SSH?

SSH (Secure Shell) is a **cryptographic network protocol** used to securely communicate with another computer over an unsecured network.

In simple terms:

> SSH lets you control another computer safely over the internet.

Without SSH, remote communication would expose:

- Passwords
- Commands
- Sensitive data

SSH solves this using encryption.

---

### 3. Why SSH is Important

SSH provides three critical guarantees:

**Confidentiality (Encryption)**  
All data is encrypted. No one can read it in transit.

**Authentication**  
Ensures you are connecting to the correct server and vice versa.

**Data Integrity**  
Prevents tampering during transmission.

Without SSH, remote administration would be dangerously insecure.

---

### 4. Key Features of SSH

**Encryption**  
Every packet is encrypted.

**Multiple Authentication Methods**

- Password-based login
- Public key authentication
- Multi-factor authentication

**Secure File Transfers**

- SCP
- SFTP

**Port Forwarding (Tunneling)**  
Securely route other protocols through SSH.

---

### 5. Core SSH Concepts

Before using SSH, understand the basic components.

**SSH Client**  
The machine initiating the connection.

**SSH Server**  
The remote machine accepting connections.

**Session**  
An encrypted communication channel.

---

### 6. How SSH Works (High-Level Flow)

When you connect via SSH:

1. Client contacts server
2. Server proves its identity (host key)
3. Encryption keys are negotiated
4. User authentication occurs
5. Secure session begins

Important idea:

> Encryption is established *before* authentication.

---

### 7. What You’ll Need

To practice SSH:

- A local machine (Windows / macOS / Linux)
- A remote server (cloud VM, Raspberry Pi, etc.)
- Terminal access

---

### 8. Installing SSH Tools

#### macOS / Linux

Most systems already include OpenSSH.

Check:

	ssh -V

If missing:

**Ubuntu / Debian**

	sudo apt update
	sudo apt install openssh-client

**macOS (Homebrew)**

	brew install openssh

---

#### Windows

Windows 10/11 includes OpenSSH.

Check in PowerShell:

	ssh -V

If missing:

Settings → Optional Features → Add Feature → OpenSSH Client

---

### 9. Your First SSH Connection

Basic syntax:

	ssh user@hostname

Example:

	ssh ubuntu@192.168.1.10

What happens:

- Server sends identity (host key)
- Client asks for verification
- Password prompt appears (if using password auth)

---

### 10. Host Key Verification (Critical Security Concept)

First-time connection:

	The authenticity of host can't be established...

This protects against:

- Man-in-the-middle attacks

Accept only if:

- You trust the server

---

### 11. Password Authentication

Simplest method:

- Enter password
- Server validates credentials

Drawback:

- Vulnerable to brute force
- Hard to automate

---

### 12. Public Key Authentication (Preferred Method)

Instead of passwords:

- You prove identity using cryptography

Two files:

- Private key (secret)
- Public key (shared)

---

### 13. What Are SSH Keys?

SSH keys are a cryptographic key pair:

**Private Key**

- Stays on your machine
- Must never be shared

**Public Key**

- Placed on server
- Grants access

---

### 14. Generating SSH Keys

Command:

	ssh-keygen

Common example:

	ssh-keygen -t ed25519 -C "your_email@example.com"

or RSA:

	ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

---

### 15. Key Type Selection

**ed25519 (Recommended)**

- Faster
- Stronger security
- Smaller keys

**RSA**

- Widely compatible
- Larger keys needed for equivalent security

---

### 16. Where Keys Are Stored

**Linux / macOS**

	~/.ssh/

**Windows**

	C:\Users\YourUser\.ssh\

---

### 17. Understanding Key Files

Example:

	id_ed25519      → Private key  
	id_ed25519.pub  → Public key

Remember:

> Losing your private key = losing access  
> Leaking your private key = losing security

---

### 18. Copying Public Key to Server

Automatic (Linux/macOS):

	ssh-copy-id user@hostname

Manual:

	cat ~/.ssh/id_ed25519.pub | ssh user@hostname "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

---

### 19. Logging in Without Password

Once keys are installed:

	ssh user@hostname

No password needed.

Why?

- Server trusts your public key
- Client proves identity using private key

---

### 20. SSH Agent (Managing Keys)

SSH agent stores decrypted keys in memory.

Start agent:

	eval "$(ssh-agent -s)"

Add key:

	ssh-add ~/.ssh/id_ed25519

Benefit:

- No repeated passphrase prompts

---

### 21. SSH Config File (Huge Productivity Boost)

File:

	~/.ssh/config

Example:

	Host myserver
	    HostName 192.168.1.10
	    User ubuntu
	    IdentityFile ~/.ssh/id_ed25519

Now connect with:

	ssh myserver

---

### 22. Secure File Transfers

**SCP**

	scp file.txt user@host:/path/

**SFTP**

	sftp user@host

---

### 23. Port Forwarding (Tunneling)

Example:

	ssh -L 8080:localhost:3000 user@host

Meaning:

- Local port 8080 → Remote port 3000

Used for:

- Databases
- Internal services
- Secure access

---

### 24. Security Best Practices

Use keys instead of passwords  
Disable root login  
Use strong passphrases  
Restrict SSH access via firewall  
Keep SSH updated

---

### 25. Troubleshooting Common Issues

**Connection refused**

- SSH server not running

**Permission denied**

- Wrong credentials
- Incorrect key permissions

Fix permissions:

	chmod 700 ~/.ssh
	chmod 600 ~/.ssh/id_ed25519

---

### 26. Advanced SSH Concepts

- SSH multiplexing
- ProxyJump / bastion hosts
- SSH tunneling chains
- Agent forwarding
- Hardware security keys

---

### 27. Final Thoughts

SSH is not just a tool — it's a foundational skill.

Mastering SSH means you can:

- Manage infrastructure
- Secure communications
- Automate deployments
- Debug remote systems

Almost every serious developer, DevOps engineer, and system administrator relies on SSH daily.

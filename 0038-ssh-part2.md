## Managing SSH Connections

### 1. Introduction

Connecting to a remote server for the first time using SSH is more than just running a command — it is a carefully designed security process.

This first interaction establishes:

- Trust between client and server
- Server identity verification
- Authentication method selection
- A secure encrypted session

Understanding this process deeply is critical for system administration, DevOps, and secure infrastructure management.

---

### 2. What Happens During a First-Time SSH Connection?

When you connect to a server you have never seen before, SSH must answer a fundamental security question:

> "How do you know this server is legitimate?"

Unlike a browser (which uses certificate authorities), SSH relies on **host keys** and **trust-on-first-use (TOFU)**.

---

### 3. Step 1 — Initiating the SSH Connection

The journey begins with the basic command:

	ssh user@hostname

Where:

- **user** → Account on the remote server
- **hostname** → IP address or domain name

Example:

	ssh johndoe@192.168.1.1

At this moment:

- Client contacts server
- No trust yet exists
- Identity must be verified

---

### 4. Step 2 — Server Identity Verification

On first connection, SSH displays a warning:

	The authenticity of host '192.168.1.1' can't be established.
	RSA key fingerprint is SHA256:xxxxxxxxxxxxxxxx
	Are you sure you want to continue connecting (yes/no)?

This is one of SSH’s most important security features.

Key idea:

> SSH does **not blindly trust servers**

---

### 5. Understanding the Fingerprint

The fingerprint is:

- A cryptographic hash
- Derived from the server’s host key
- Unique to that server

Think of it as:

> A digital identity signature of the server

If provided by your administrator, you should compare it.

---

### 6. Trust On First Use (TOFU)

If you type:

	yes

SSH will:

- Store the server’s public key locally
- Add it to `~/.ssh/known_hosts`

From now on:

- Server identity is remembered
- No more prompts (unless key changes)

---

### 7. Why This Step Matters (Security Insight)

This prevents:

**Man-in-the-Middle Attacks**

Without verification:

- Attacker could impersonate server
- Capture credentials
- Intercept commands

SSH actively protects you here.

---

### 8. Step 3 — Authentication Phase

After trust is established, SSH moves to:

> "Who are you?"

Two common possibilities:

**1. Public Key Authentication**  
**2. Password Authentication**

---

### 9. Case 1 — Public Key Authentication (Preferred)

If your key is already installed on the server:

- No password prompt
- Automatic login

Why?

- Client proves identity cryptographically
- Server validates against authorized keys

---

### 10. Case 2 — Password Authentication

If keys are not configured:

	user@192.168.1.1's password:

Server validates:

- Username
- Password

Drawbacks:

- Less secure
- Vulnerable to brute force
- Not automation-friendly

---

### 11. Step 4 — Session Established

After successful authentication:

- Encrypted channel fully active
- Shell access granted
- Commands executable

You are now operating remotely.

---

## Deep Dive — SSH Authentication Workflow

---

### 12. High-Level Authentication Flow

Let’s break the mechanism precisely.

1. Client initiates connection
2. Server presents host key
3. Client verifies known_hosts
4. Encryption negotiated
5. Authentication begins
6. Access granted or denied

Important distinction:

> Server identity verification ≠ User authentication

---

### 13. The Cryptographic Challenge Mechanism

With key-based authentication:

- Server issues challenge
- Client signs using private key
- Server verifies using public key

No passwords transmitted.

This is why SSH keys are extremely secure.

---

## Critical SSH Files Explained

---

### 14. ~/.ssh/authorized_keys (Server-Side)

This file lives on:

> The remote server

Purpose:

> Defines WHO is allowed to log in via keys

---

### 15. What Does authorized_keys Contain?

Each line represents:

- One public key
- One trusted identity

Example:

	ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBxExampleKeyData user@example.com

Structure:

- Key type
- Key data
- Optional comment

---

### 16. How authorized_keys Works

During login:

- Server checks this file
- Looks for matching public key
- Validates signature

If match:

✔ Access granted  
If no match:

✘ Permission denied

---

### 17. Is authorized_keys Required?

For key-based authentication:

✔ Yes — absolutely required

Without it:

- Server cannot validate your identity

---

### 18. ~/.ssh/known_hosts (Client-Side)

This file lives on:

> Your local machine

Purpose:

> Defines WHICH servers you trust

---

### 19. What Does known_hosts Contain?

Each line:

- Server hostname/IP
- Server public key

Example:

	192.168.1.1 ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIExampleHostKey

---

### 20. How known_hosts Works

On connection:

- Client checks known_hosts
- Compares server key

If match:

✔ Safe to proceed

If mismatch:

⚠ SECURITY WARNING

---

### 21. When Do You See Warnings?

Example:

	WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!

Possible causes:

- Server reinstalled
- Legitimate key rotation
- Potential attack

Never ignore blindly.

---

### 22. Is known_hosts Required?

Technically:

✘ Not mandatory

Practically:

✔ Extremely important for security

It prevents impersonation attacks.

---

---

## Relationship Between authorized_keys and known_hosts

---

### 23. Different Roles, Different Responsibilities

**authorized_keys → Server Trusts Users**  
**known_hosts → Client Trusts Servers**

They solve two different security problems.

---

### 24. Authentication vs Verification

**Server Verification**

- Handled by known_hosts
- Prevents fake servers

**User Authentication**

- Handled by authorized_keys
- Prevents unauthorized users

Both are critical for full security.

---

### 25. Do These Files Always Participate?

**authorized_keys**

✔ Always checked during key authentication

**known_hosts**

✔ Checked during server verification  
✘ Can be bypassed (not recommended)

---

## Practical Security Best Practices

---

### 26. Always Verify Fingerprints for Critical Systems

For:

- Production servers
- Sensitive infrastructure
- Corporate environments

Never rely purely on TOFU.

---

### 27. Protect authorized_keys on Server

Best practices:

- Correct permissions

	chmod 700 ~/.ssh
	chmod 600 ~/.ssh/authorized_keys

- Avoid accidental edits
- Monitor unauthorized additions

---

### 28. Protect known_hosts on Client

Never casually delete unless necessary.

Why?

- Removes server trust history
- Reintroduces MITM risk

---

## Common First-Time Connection Issues

---

### 29. Connection Refused

Cause:

- SSH server not running
- Firewall blocking port 22

---

### 30. Permission Denied

Cause:

- Wrong username
- Wrong key
- Missing authorized_keys entry
- Incorrect permissions

---

### 31. Host Key Changed Warning

Correct response:

✔ Investigate first  
✔ Confirm with administrator  
✘ Do NOT blindly remove

---

## Advanced Concepts (Professional-Level Understanding)

---

### 32. Host Key Algorithms

Servers may use:

- RSA
- ECDSA
- ED25519

Modern recommendation:

✔ ED25519 preferred

---

### 33. Strict Host Key Checking

Configurable behavior:

	ssh -o StrictHostKeyChecking=yes user@host

Used in:

- Automation
- CI/CD pipelines
- Security-sensitive workflows

---

### 34. Key Rotation & Infrastructure Changes

In real systems:

- Host keys may change
- Servers replaced
- Scaling environments dynamic

Requires:

- Proper key management strategy

---

### 35. Final Thoughts

A first-time SSH connection is fundamentally about:

✔ Establishing trust  
✔ Verifying identity  
✔ Enforcing authentication  
✔ Preventing impersonation  

SSH’s design is deeply security-focused.

Understanding this workflow transforms SSH from:

"A simple login tool"

into

"A robust cryptographic security system"

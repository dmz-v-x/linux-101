# systemd Service Unit Files — How Linux Services Work Internally

## What is a Service Unit File?

A **unit file** is a configuration file used by systemd to manage services.

It tells systemd:

✔ What to run  
✔ How to run it  
✔ When to run it  
✔ What it depends on  
✔ How to recover from failure  

---

## Where Unit Files Live

Common locations:

/etc/systemd/system  
/lib/systemd/system  
/usr/lib/systemd/system  

---

### Important Distinction

System-provided services → /lib or /usr/lib  
Custom/user services → /etc/systemd/system  

---

## Basic Structure of a Unit File

A unit file is divided into sections.

Example skeleton:

[Unit]  
[Service]  
[Install]

Each section has a specific purpose.

---

# The [Unit] Section — Service Metadata & Dependencies

---

## Purpose of [Unit]

Defines:

✔ Description  
✔ Startup ordering  
✔ Dependencies  

---

## Example

[Unit]  
Description=My Application  
After=network.target  

---

## Key Directives

---

### Description

Human-readable explanation.

Used for clarity only.

---

### After / Before (Critical)

Controls startup ordering.

Example:

After=network.target

Meaning:

Start this service only after networking is ready.

Important:

Ordering ≠ dependency guarantee

---

### Requires (Stronger Relationship)

Example:

Requires=postgresql.service

Meaning:

✔ If PostgreSQL fails → this service stops  
✔ Hard dependency  

---

### Wants (Weaker Relationship)

Example:

Wants=redis.service

Meaning:

✔ Try to start Redis  
✔ Failure tolerated  

---

## The [Service] Section — Core Behavior Definition

This is the heart of the unit file.

Defines:

✔ What actually runs  
✔ Execution rules  
✔ Restart behavior  

---

## ExecStart — Most Important Directive

---

### What ExecStart Defines

The command systemd executes.

Example:

ExecStart=/usr/bin/python /opt/app.py

Meaning:

✔ Run python  
✔ Pass script as argument  

---

### Critical Insight

ExecStart behaves like:

Shell command execution

But controlled by systemd.

---

### Common Beginner Mistake

Incorrect path:

ExecStart=python app.py

Fails because:

✔ systemd requires full executable paths  

Correct:

ExecStart=/usr/bin/python /path/app.py

---

## Type — Process Startup Behavior (Advanced Insight)

Defines how systemd interprets service startup.

Common types:

simple (default)  
forking  
oneshot  

---

### simple

Process runs directly.

Most modern services use this.

---

### forking

Used by traditional daemons.

Process forks → parent exits → child continues.

---

### oneshot

Short-lived tasks.

Example:

Initialization scripts.

---

## Restart Policies — Reliability Mechanism

One of systemd’s most powerful features.

---

### Restart Directive

Example:

Restart=on-failure

Meaning:

✔ Restart service if it crashes  

---

### Common Restart Options

no → default  
on-failure → restart on crash  
always → restart regardless  
on-abnormal → restart on unexpected termination  

---

### Why Restart Policies Matter

Provide:

✔ Self-healing systems  
✔ Crash recovery  
✔ Stability  

Example:

Web server crash → automatic restart.

---

## RestartSec — Restart Delay

Example:

RestartSec=5

Meaning:

Wait 5 seconds before restart.

Prevents rapid crash loops.

---

## User Directive — Security Context

Example:

User=www-data

Meaning:

Run service as non-root user.

Critical for security.

---

## WorkingDirectory — Execution Context

Example:

WorkingDirectory=/opt/myapp

Ensures correct relative paths.

---

## The [Install] Section — Boot Behavior

Defines:

✔ When service starts at boot  
✔ Target integration  

---

## WantedBy — Boot Target Association

Example:

WantedBy=multi-user.target

Meaning:

✔ Start service in normal system mode  

---

### Why Install Section Matters

systemctl enable uses this section.

Without it:

✔ Service cannot be enabled  

---

## Putting Everything Together — Example Unit File

---

[Unit]  
Description=My Python App  
After=network.target  

[Service]  
Type=simple  
ExecStart=/usr/bin/python /opt/app.py  
Restart=on-failure  
RestartSec=3  
User=himanshu  
WorkingDirectory=/opt  

[Install]  
WantedBy=multi-user.target  

---

## Dependencies — Deep Understanding

Dependencies control:

✔ Startup ordering  
✔ Failure relationships  
✔ Service coupling  

---

## Ordering vs Requirement

After=network.target → ordering  
Requires=network.target → strict dependency  

Critical distinction.

---

## Restart Policies — Deep System Impact

Restart policies influence:

✔ System resilience  
✔ Failure recovery  
✔ Availability  

Misconfiguration may cause:

✔ Infinite restart loops  
✔ System instability  

---

## Debugging Unit Files (Expert Skill)

---

## Common Failure Causes

---

### Incorrect ExecStart Path

Most frequent error.

---

### Missing Dependencies

Service starts too early.

Fails.

---

### Permission Issues

Wrong User directive.

---

### Restart Storms

Improper Restart settings.

---

## Debugging Commands

systemctl status service_name  
journalctl -u service_name  

---

## Advanced Insights

---

## systemd Tracks Process via PID

systemd monitors:

✔ Main PID  
✔ Child processes  
✔ Exit status  

---

## systemd is a Process Supervisor

Not just launcher.

Manages:

✔ Lifecycle  
✔ Restart logic  
✔ Failure detection  

---

## Unit Files = Declarative Service Model

Describe desired state.

systemd enforces it.

---

## Common Beginner Confusions

---

## “Why full paths required?”

systemd ≠ interactive shell.

No PATH resolution.

---

## “After guarantees dependency?”

Incorrect.

Ordering only.

---

## “Restart always safe?”

Dangerous if service crashes repeatedly.

---

## “Why User directive important?”

Security isolation.

Principle of least privilege.

---

## Expert-Level Perspective

Service unit files represent:

✔ Service architecture blueprint  
✔ Reliability policy  
✔ Dependency graph  
✔ Security boundary  

Mastering unit files means mastering Linux services.

---

## Final Mental Model

Think of unit files as:

Instruction manuals for systemd.

They define:

✔ What systemd should do  
✔ How systemd should react  
✔ How systemd maintains stability  

---

## Key Takeaways

- Unit files define service behavior
- Structured into [Unit], [Service], [Install]
- ExecStart specifies execution command
- Full executable paths required
- Dependencies control startup relationships
- Restart policies provide resilience
- RestartSec prevents crash loops
- User directive enforces security context
- WantedBy controls boot integration
- Unit files are foundation of systemd services

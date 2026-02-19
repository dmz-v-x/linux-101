# Viewing Running Processes in Linux

## What Does `ps` Actually Do?

The `ps` command displays information about **currently running processes**.

Important clarification:

`ps` does NOT show “programs”.

It shows **processes** (running instances).

---

## Basic Command — `ps`

### What Happens When You Run It

Typing:

ps

Displays processes associated with your **current terminal session**.

Typical output:

PID   TTY      TIME   CMD  
2451  pts/0    00:00  bash  
3012  pts/0    00:00  ps  

---

### Why Output is Limited

By default, `ps` shows:

✔ Only processes tied to your terminal  
✔ Not system-wide processes  

This surprises many beginners.

---

## Extended View — `ps aux`

This is the most commonly used variant.

---

### What `ps aux` Shows

Typing:

ps aux

Displays:

✔ All processes  
✔ All users  
✔ System-wide view  

This includes:

- System services
- Background daemons
- Other users’ processes

---

### Example Output Structure

USER   PID  %CPU  %MEM  VSZ   RSS   TTY   STAT   START   TIME   COMMAND

Each column has important meaning.

We now decode them carefully.

---

## Understanding Key Columns (Critical Skill)

---

## USER — Who Owns the Process

### What USER Means

Indicates the user account that started the process.

Examples:

root  
himanshu  
mysql  

---

### Why USER Matters

Helps identify:

✔ Permission boundaries  
✔ Security context  
✔ Responsibility for resource usage  

Example:

root processes → system-level  
user processes → session-level  

---

## PID — Process Identifier

### What PID Means

Unique numeric identifier assigned by the kernel.

Example:

PID → 2451  
PID → 9834  

---

### Why PID Matters

Used for:

✔ Killing processes  
✔ Monitoring resource usage  
✔ Debugging issues  

Every process has exactly one PID.

---

## %CPU — CPU Usage

### What %CPU Means

Indicates how much CPU time the process is consuming.

Interpretation:

0.0 → idle  
5.0 → moderate usage  
90.0 → heavy usage  

---

### Important Insight

High CPU usage may indicate:

✔ Heavy computation  
✔ Infinite loops  
✔ Performance bottlenecks  

---

## %MEM — Memory Usage

### What %MEM Means

Percentage of RAM used by the process.

Helps detect:

✔ Memory-heavy applications  
✔ Potential leaks  
✔ System pressure  

---

### Why Memory Monitoring Matters

Memory exhaustion leads to:

✔ System slowdown  
✔ Swapping  
✔ OOM (Out Of Memory) kills  

---

## TTY — Terminal Association

### What TTY Means

Indicates which terminal controls the process.

Examples:

pts/0 → pseudo terminal  
tty1 → physical console  
? → no terminal  

---

### Why TTY Matters

Helps distinguish:

✔ Interactive processes  
✔ Background daemons  

Daemon processes typically show:

TTY → ?

Meaning:

No terminal attached.

---

## STAT — Process State (Extremely Important)

One of the most valuable columns.

---

### Common STAT Values

R → Running  
S → Sleeping  
D → Uninterruptible Sleep  
T → Stopped  
Z → Zombie  

---

### Why STAT is Critical

Reveals:

✔ What the process is doing  
✔ Whether it is stuck  
✔ Whether it is healthy  

---

### Advanced STAT Flags

You may see combinations:

Ss  
Sl  
R+  

Extra letters provide deeper hints:

s → session leader  
l → multi-threaded  
+ → foreground process  

---

## TIME — CPU Time Consumed

### What TIME Means

Total CPU time used since process start.

Not wall-clock time.

Example:

00:00 → minimal CPU work  
10:32 → significant CPU usage  

---

## COMMAND — Executed Program

### What COMMAND Shows

Displays:

✔ Program name  
✔ Arguments  
✔ Execution details  

Example:

python app.py  
nginx: worker process  
sshd: user session  

---

## Alternative Format — `ps -ef`

Another widely used variant.

---

## What `ps -ef` Displays

Typing:

ps -ef

Shows processes in a different style.

Example columns:

UID   PID   PPID   C   STIME   TTY   TIME   CMD

---

## Why Multiple Formats Exist

Different views emphasize different information.

---

### `ps aux`

Better for:

✔ Resource monitoring  
✔ CPU / Memory focus  

---

### `ps -ef`

Better for:

✔ Process hierarchy  
✔ Parent-child tracing  

---

## Key Additional Column — PPID

### What PPID Means

Parent Process ID.

Indicates which process created this one.

Example:

PID → 3012  
PPID → 2451  

Meaning:

Process 2451 created process 3012.

---

### Why PPID Matters

Helps diagnose:

✔ Zombie processes  
✔ Orphan processes  
✔ Process chains  

---

## Choosing the Right `ps` Command

---

## Use `ps` When

✔ Checking terminal processes  
✔ Simple inspection  

---

## Use `ps aux` When

✔ Viewing everything  
✔ Monitoring CPU / Memory  
✔ Diagnosing system load  

---

## Use `ps -ef` When

✔ Tracing process trees  
✔ Debugging parent-child relationships  

---

## Advanced Usage Patterns

---

## Searching for Specific Processes

Example:

ps aux | grep nginx

Helps locate:

✔ Specific applications  
✔ Stubborn processes  

---

## Sorting by CPU Usage

Example conceptually:

Identify heavy CPU consumers.

---

## Detecting Zombies

Look for:

STAT → Z

---

## Identifying Daemons

Look for:

TTY → ?

---

## Common Beginner Confusions

---

## “Why doesn’t `ps` show everything?”

Because default view is terminal-limited.

---

## “Why are there so many processes?”

Linux is multi-process by design.

System services, background tasks, user programs all run as processes.

---

## “What does STAT = S mean?”

Sleeping — usually normal.

---

## “Is high CPU always bad?”

Not necessarily.

Could be legitimate computation.

Pattern & context matter.

---

## Expert-Level Perspective

The `ps` command is not just informational.

It is a diagnostic instrument.

Reading process tables reveals:

✔ System health  
✔ Performance issues  
✔ Resource pressure  
✔ Application behavior  
✔ Scheduling effects  

Experienced engineers mentally interpret `ps` output like reading vital signs.

---

## Key Takeaways

- `ps` shows running processes
- Default `ps` is terminal-limited
- `ps aux` shows system-wide processes
- `ps -ef` emphasizes hierarchy
- USER identifies ownership
- PID uniquely identifies processes
- %CPU reveals CPU consumption
- %MEM reveals memory usage
- TTY shows terminal association
- STAT reveals process state
- COMMAND shows executed program


# What Happens When You Run a Command

## The Simple User Perspective

You type:

ls

And output appears.

From the outside, it looks instant.

From the inside, many things just happened.

---

## High-Level Overview

When you run a command:

- The shell interprets your input
- The executable is located
- The program is loaded into RAM
- A process is created
- A PID is assigned
- Parent / child relationships are formed
- CPU begins execution

We now examine each stage carefully.

---

## Step 1 — Shell Receives Your Command

Linux does not directly execute your typing.

Your **shell** (bash, zsh, sh, etc.) receives the input.

Example:

You type:

ls

The shell reads this as text.

At this point:

Nothing is running yet.

---

## Step 2 — Command Resolution (Finding the Executable)

The shell must locate the program file.

It searches directories listed in the PATH variable.

Example PATH:

/usr/local/bin:/usr/bin:/bin

The shell checks:

- /usr/local/bin/ls
- /usr/bin/ls
- /bin/ls

Eventually it finds:

/bin/ls

Important insight:

You are not executing "ls".

You are executing a file stored somewhere on disk.

---

## Step 3 — Execution Request to the Kernel

The shell cannot run programs itself.

It asks the kernel:

“Please execute this file.”

This happens via a system call.

Now the kernel takes control.

---

## Step 4 — Process Creation Begins

The kernel creates a new process.

But this is not done from scratch.

Linux uses a mechanism called:

fork()

---

## Step 5 — fork(): Creating a Child Process

The shell is already a running process.

Instead of building a process from nothing, the kernel:

- Clones the shell process
- Creates a child process
- Assigns new PID

Now we have:

Parent Process → Shell  
Child Process → Future ls

### Why fork() Exists

fork():

- Is extremely efficient
- Copies memory logically (copy-on-write)
- Preserves environment

This design makes process creation fast.

---

## Step 6 — Parent / Child Relationship

After fork():

- Shell = Parent
- New process = Child

This relationship is critical.

The parent:

- Can monitor child
- Can wait for completion
- Can manage signals

---

## Step 7 — exec(): Replacing Child Memory

The child process is initially a copy of the shell.

But it must become ls.

So it calls:

exec()

exec():

- Discards old memory contents
- Loads new executable (/bin/ls)
- Keeps same PID

Important insight:

fork() creates process  
exec() transforms process

---

## Step 8 — Executable Loaded into RAM

Now the kernel loads the program.

This includes:

- Machine instructions (code segment)
- Static data
- Required libraries
- Stack setup
- Heap initialization

At this moment:

The program becomes a running process.

---

## Step 9 — PID Assignment

Every process receives a unique:

PID (Process ID)

Why PID Matters:

- Kernel tracks processes via PID
- Signals sent using PID
- Resource usage tracked per PID

Example:

Shell PID → 2450  
ls PID → 3012

---

## Step 10 — Execution State Initialization

The kernel sets up:

- CPU registers
- Program counter
- Stack pointer
- Scheduling metadata

Now the process is ready to run.

---

## Step 11 — CPU Scheduling

The CPU does not immediately run ls.

Linux scheduler decides:

- When it runs
- How long it runs
- Priority level

Processes compete for CPU time.

---

## Step 12 — Program Execution Begins

CPU starts executing instructions from /bin/ls.

Now:

- Disk may be accessed
- Memory used
- Output generated

---

## Step 13 — Interaction with System Resources

During execution, the process may:

- Open files
- Read directories
- Allocate memory
- Write output to terminal

All of these are mediated by the kernel.

Processes never directly access hardware.

---

## Step 14 — Process Completion

Once ls finishes:

- Process exits
- Kernel releases memory
- Exit status returned to shell

---

## Step 15 — Parent Resumes Control

The shell:

- Receives exit code
- Displays prompt again
- Waits for next command

Cycle complete.

---

## Deep Dive — Critical Internal Concepts

---

## Why fork() + exec() Design?

This design provides:

- Efficiency
- Flexibility
- Isolation
- Stability

Processes can be created rapidly with minimal overhead.

---

## Copy-on-Write Optimization

fork() does NOT immediately copy memory.

Instead:

- Parent & child share pages
- Copy occurs only on modification

This makes process creation extremely fast.

---

## Process Metadata Maintained by Kernel

Kernel tracks:

- PID
- Memory mappings
- Open files
- CPU usage
- Scheduling state
- Parent / child links

---

## Parent Waiting Mechanism

Shell often calls:

wait()

This:

- Pauses parent
- Waits for child completion
- Prevents zombie processes

---

## What If Parent Does Not Wait?

Child may become:

Zombie process

Zombie = Process finished but not cleaned up.

---

## Real-World Example Flow

---

## Example: Running a Python Script

Command:

python app.py

Flow:

Shell → fork → child  
Child → exec python  
Python process → loads app.py  
Python interpreter → executes script

Multiple layers of execution occur.

---

## Example: Running Background Command

sleep 100 &

Flow:

Shell → fork → exec sleep  
Shell does NOT wait  
Process runs independently

---

## Example: Pipeline Execution

ls | grep txt

Flow:

Shell creates MULTIPLE processes:

- ls process
- grep process

Connected via pipe mechanism.

---

## Common Beginner Misconceptions

---

## “Does shell run the program?”

No.

Kernel runs programs.

Shell only requests execution.

---

## “Does exec() create a process?”

No.

exec() replaces memory of an existing process.

---

## “Why is fork() needed?”

Because creating processes from scratch is expensive.

fork() is optimized.

---

## “Why each process has PID?”

For tracking, scheduling, signaling, resource management.

---

## Expert-Level Perspective

Running a command is essentially:

- Process cloning
- Memory replacement
- Resource allocation
- Scheduler integration

Every command is a full process lifecycle event.

This mechanism powers:

- Services
- Daemons
- Background jobs
- Pipelines
- Multi-process applications

---

## Final Mental Model

Typing a command triggers:

Shell → Kernel → fork → exec → Process → Scheduler → CPU

What feels instant is actually a highly coordinated system operation.

---

## Key Takeaways

- Shell interprets commands
- PATH resolves executable location
- Kernel handles execution
- fork() creates child process
- exec() loads executable
- Executable loaded into RAM
- PID assigned
- Parent / child relationships formed
- Scheduler manages execution
- Process runs and exits

---

This understanding is foundational for:

- Process control
- Debugging
- System performance analysis
- Service management
- Advanced Linux internals

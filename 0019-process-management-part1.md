# Program vs Process

## What is a Program?

A **program** is simply a file stored on disk.

It is passive and inactive.

Examples of program files:

- /usr/bin/python  
- /usr/bin/nginx  
- /bin/ls  

These are just binary files containing machine instructions.

Nothing is running yet.

### Key Properties of a Program

A program:

- Exists on disk
- Is static (not executing)
- Has no PID
- Uses no CPU
- Uses no RAM

A program does absolutely nothing until it is executed.

### Mental Model

Think of a program as:

A recipe written in a cookbook.

Instructions exist, but no cooking is happening.

---

## What is a Process?

A **process** is a running instance of a program.

When a program is executed, the operating system transforms it into a process.

Now the instructions are actively being executed by the CPU.

### Key Properties of a Process

A process:

- Lives in RAM
- Has a PID (Process ID)
- Consumes CPU cycles
- Uses memory
- Has execution state

### Mental Model

Think of a process as:

Someone actively cooking using the recipe.

Now work is happening.

---

## Program vs Process — Direct Comparison

| Program | Process |
|----------|----------|
| File on disk | Running entity |
| Passive | Active |
| No PID | Has PID |
| No CPU usage | Uses CPU |
| No memory usage | Uses RAM |
| Static | Dynamic |

---

## What Happens When You Run a Program?

When you execute a command like:

python script.py

Several internal steps occur.

### Step 1 — Program Exists on Disk

/usr/bin/python is just a file.

Nothing is running.

---

### Step 2 — Execution Request

The shell asks the kernel to execute the program.

---

### Step 3 — Kernel Creates Process

The kernel:

- Allocates memory
- Loads executable code into RAM
- Assigns a PID
- Sets up execution context

Now we have a process.

---

### Step 4 — CPU Begins Execution

The CPU starts running the instructions.

Now system resources are consumed.

---

## Multiple Processes from the Same Program

One program file can create many processes.

Example:

Open multiple terminals and run:

python

Each instance:

- Uses the same program file
- Creates a new process
- Gets a unique PID
- Has its own memory space

### Why This Works

Because:

Program = blueprint  
Process = independent execution

Each process is isolated.

They do not share runtime memory.

---

## Why This Distinction Matters

This is not theoretical — it affects real system behavior.

---

### Memory Usage

A program on disk uses zero RAM.

A process:

- Allocates memory
- Maintains stack
- Maintains heap
- Stores runtime data

Processes consume memory.

---

### CPU Scheduling

The CPU schedules processes, not programs.

The scheduler:

- Selects which process runs
- Allocates time slices
- Applies priorities

---

### Isolation & Stability

Each process:

- Has its own memory space
- Cannot directly access another process’s memory

If one crashes, others survive.

---

## Advanced View — What a Process Actually Contains

A process is far more than “a running program”.

It includes:

- Code (text segment)
- Data segment
- Heap
- Stack
- CPU registers
- File descriptors
- Environment variables
- Scheduling metadata
- PID / PPID

A process is a complete execution environment.

---

## Forking — How Processes Multiply

Processes can create other processes.

Linux uses the fork mechanism.

### fork()

fork():

- Creates a child process
- Copies parent memory (logically)
- Assigns new PID

Example:

Shell → fork → ls process

Important insight:

Programs do not create processes.

Processes create processes.

---

## Exec — Replacing Process Memory

After fork(), processes often call exec().

exec():

- Replaces process memory
- Loads new program code

Same process → new program execution.

---

## Real-World Examples

---

### Web Servers

Program on disk:

nginx binary

Processes in memory:

- Master process
- Worker processes

One program → multiple processes.

---

### Browsers

Program:

Chrome executable

Processes:

- Tab processes
- Renderer processes
- GPU process

Modern applications heavily rely on multi-process design.

---

### Python Interpreter

Program:

python binary

Processes:

- Separate script executions
- Independent runtime states

---

## Common Beginner Confusions

---

### “Why do I see many nginx entries?”

Because one program can spawn multiple processes.

---

### “If I delete the program file, does the process die?”

No.

Process already loaded into RAM.

---

### “Why doesn’t killing one process kill others?”

Because processes are isolated.

---

## Expert-Level Perspective

At system design level:

Program = static artifact  
Process = dynamic execution unit

Processes represent:

- Scheduling units
- Resource accounting units
- Isolation boundaries
- Failure boundaries

Everything the OS manages revolves around processes.

---

## Final Mental Model

A program is like:

A blueprint stored in a drawer.

A process is like:

A factory actively running using that blueprint.

---

## Key Takeaways

- Program = file on disk
- Process = running instance
- Program is passive
- Process is active
- One program → many processes
- Processes consume CPU & RAM
- Kernel schedules processes
- Processes provide isolation

---

This distinction is foundational for:

- Process management
- Performance tuning
- Services & daemons
- Debugging
- System stability

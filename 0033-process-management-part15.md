# CPU & Memory Resource View in Linux 

## Why Resource Monitoring Matters

Your system runs many processes simultaneously.

Each process consumes:

- CPU cycles
- Memory
- IO bandwidth

When resources become constrained:

✔ System slows down  
✔ Applications lag  
✔ Processes compete  
✔ Bottlenecks appear  

Monitoring tools reveal these pressures.

---

## Viewing CPU & Memory with `top`

---

## Why `top` is the Primary Tool

`top` provides a live, continuously updating system view.

It shows:

- CPU usage
- Memory usage
- Load average
- Process-level consumption

---

## Running `top`

Type:

top

Two sections are most important:

1. CPU summary  
2. Memory summary  

---

## CPU Consumption

Example CPU line:

%Cpu(s): 12.5 us, 3.0 sy, 82.0 id, 2.5 wa

Each component matters.

---

### us — User CPU Time

Time spent running user applications.

High value may indicate:

✔ Heavy computation  
✔ CPU-intensive tasks  

---

### sy — System CPU Time

Kernel operations.

High value may indicate:

✔ Excessive system calls  
✔ Kernel overhead  
✔ IO-heavy workloads  

---

### id — Idle CPU

Unused CPU capacity.

Low idle → CPU bottleneck possible.

---

### wa — IO Wait

CPU waiting for IO.

High wa often means:

✔ Disk bottleneck  
✔ Storage latency  
✔ Network filesystem delays  

---

## Key CPU Diagnostic Insight

High CPU usage alone is not enough.

Interpretation requires:

✔ Idle percentage  
✔ Load average  
✔ IO wait  

---

## Memory Usage in `top`

Example:

MiB Mem : 16000 total, 1200 free, 9000 used, 5800 buff/cache

---

### used — Memory Actively Used

Memory consumed by processes.

---

### free — Completely Unused Memory

Usually small in Linux.

Normal behavior.

---

### buff/cache — Cached Memory

Important concept:

Cached memory is reclaimable.

Linux uses free RAM aggressively for caching.

---

## Critical Beginner Insight

Low "free memory" is NOT necessarily bad.

Linux prefers:

✔ Use RAM for performance  
✔ Reclaim when needed  

---

## Viewing Processes with `ps`

---

## Why `ps` Matters for Resource Analysis

`top` shows live usage.

`ps` shows structured snapshots.

---

## Useful Command

ps aux

Key columns:

- %CPU
- %MEM
- PID
- COMMAND

---

## CPU Consumption Per Process

Example:

%CPU → identifies heavy consumers.

Helps detect:

✔ Runaway processes  
✔ Infinite loops  
✔ Resource hogs  

---

## Memory Consumption Per Process

%MEM → reveals RAM-heavy applications.

Helps detect:

✔ Memory leaks  
✔ Inefficient workloads  
✔ Oversized processes  

---

## Understanding System Memory with `free`

---

## What `free` Displays

Type:

free -h

Example output:

              total   used   free   shared  buff/cache  available

---

## Why `free` is Important

Provides system-level RAM overview.

Cleaner than `top` for memory-specific analysis.

---

## Most Important Column — available

Represents:

✔ Memory usable without swapping  

More meaningful than "free".

---

## Key Memory Diagnostic Insight

Low available memory → memory pressure.

Possible consequences:

✔ Swapping  
✔ System slowdown  
✔ OOM killer  

---

## System Statistics with `vmstat`

---

## What `vmstat` Shows

Type:

vmstat 1

Updates every second.

Displays:

- CPU metrics
- Memory metrics
- IO activity
- Process queues

---

## Critical Fields for Diagnostics

---

### r — Run Queue

Processes waiting for CPU.

High value → CPU contention.

---

### free — Free Memory

Unused RAM.

---

### si / so — Swap Activity

Swap In / Swap Out.

Non-zero values indicate:

✔ Memory pressure  
✔ Disk-backed memory usage  
✔ Performance degradation  

---

### us / sy / id / wa — CPU Breakdown

Similar to `top`.

---

## CPU Consumption — Deep Understanding

---

## CPU is a Shared Resource

High CPU usage means:

✔ Processes actively executing  
✔ Scheduler heavily engaged  

But interpretation requires context.

---

## CPU Bottleneck Indicators

✔ High load average  
✔ Low idle CPU  
✔ High run queue (vmstat r)  

---

## Memory Usage — Deep Understanding

---

## Memory Pressure Indicators

✔ Low available memory  
✔ High swap activity  
✔ Increasing so/si values  

---

## Why Swapping is Dangerous

Swap uses disk → slow compared to RAM.

Leads to:

✔ Severe latency  
✔ System sluggishness  
✔ Thrashing  

---

## Bottleneck Detection (Most Important Skill)

Resource monitoring ultimately answers:

“What is slowing the system?”

---

## CPU Bottleneck

Symptoms:

✔ High load  
✔ Low idle CPU  
✔ Many processes waiting (vmstat r)  

---

## Memory Bottleneck

Symptoms:

✔ High swap usage  
✔ Low available memory  
✔ System pauses  

---

## IO Bottleneck

Symptoms:

✔ High IO wait (wa)  
✔ Processes stuck in D state  
✔ Low CPU usage but slow system  

---

## Diagnostic Patterns (Expert-Level Thinking)

---

## Pattern 1 — System Feels Slow

Check:

top → CPU idle → IO wait → Load average

---

## Pattern 2 — Applications Freezing

Check:

Memory pressure → swap activity → available memory

---

## Pattern 3 — CPU Spikes

Check:

Sort processes by %CPU.

---

## Pattern 4 — Memory Leak Suspicion

Check:

ps aux → sort by %MEM.

---

## Pattern 5 — Hidden Bottleneck

High load + idle CPU → IO issue likely.

---

## Common Beginner Misconceptions

---

## “Free memory must be high”

Incorrect.

Linux uses RAM for caching.

---

## “High CPU always bad”

Incorrect.

Depends on workload legitimacy.

---

## “Low CPU means healthy system”

Incorrect.

Check load & IO wait.

---

## “Swap usage normal”

Heavy swap usage → performance warning.

---

## Expert-Level Perspective

Resource monitoring tools reveal:

✔ System health  
✔ Workload behavior  
✔ Contention points  
✔ Capacity limits  
✔ Performance risks  

Experienced engineers correlate:

CPU + Memory + IO + Load

Not isolated metrics.

---

## Final Mental Model

System performance depends on balance.

CPU pressure → scheduling delays  
Memory pressure → swapping delays  
IO pressure → waiting delays  

Monitoring tools expose imbalance.

---

## Key Takeaways

- CPU & memory are critical resources
- `top` provides live resource view
- `ps` shows per-process consumption
- `free` clarifies memory availability
- `vmstat` reveals deeper system dynamics
- CPU bottlenecks show high load & low idle
- Memory bottlenecks show swap activity
- IO bottlenecks show high IO wait
- Resource interpretation requires context
- Bottleneck detection is core Linux skill

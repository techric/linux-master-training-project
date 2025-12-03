# Module 01: Advanced Process Debugging and Inspection (The Other 5%)

## Overview

This module covers deep process-level diagnostics—tools and techniques used when an application is hung, behaving incorrectly, leaking memory, locking files, or consuming unexpected resources. These commands are not used every day, but when they are needed, they are essential.

You will learn how to:

- Capture real-time system call activity  
- Inspect open files, sockets, pipes, and locks  
- Analyze process stacks  
- Debug hung or zombie processes  
- Combine multiple tools to perform root-cause analysis  

---

## Learning Objectives

By the end of this module, you should be able to:

- Use `ps` with advanced filters and formatting  
- Use `lsof` to discover file handles, ports, and locks  
- Use `strace` to trace a running process  
- Use `pstack` or `gdb` to review stack traces  
- Diagnose frozen, hung, or defunct processes  

---

## Lesson 1: Advanced ps Usage

The standard `ps aux` is useful, but complex issues require more precision.

List processes owned by a specific user:
    ps -u username

Sort by CPU usage:
    ps aux --sort=-%cpu | head

Sort by memory usage:
    ps aux --sort=-%mem | head

Show PID, PPID, CPU, memory, and command:
    ps -eo pid,ppid,%cpu,%mem,cmd | head

Filter by process name:
    ps aux | grep appname

This is the foundation for selecting the right process for deeper debugging.

---

## Lesson 2: lsof — List Open Files and Sockets

`lsof` shows everything a process has opened:

List all open files:
    sudo lsof

Show all open files for a specific PID:
    sudo lsof -p 1234

Show what process is using a file:
    sudo lsof /var/log/syslog

Find which process is listening on a port:
    sudo lsof -iTCP:443 -sTCP:LISTEN

Find which process is holding a deleted file (common with log rotation issues):
    sudo lsof | grep deleted

This is one of the most powerful diagnostic tools on Linux.

---

## Lesson 3: strace — System Call Tracing

`strace` shows what a process is doing at the system-call level. Use it for:

- Hung processes  
- Unknown errors  
- Permission problems  
- Infinite loops  
- Network stalls  

Trace a command from start:
    strace -f -o trace.log mycommand

Attach to a running process:
    sudo strace -p 1234

Trace only file operations:
    sudo strace -p 1234 -e trace=file

Trace network-related syscalls:
    sudo strace -p 1234 -e trace=network

Trace time-related syscalls:
    sudo strace -p 1234 -e trace=clock

Use `strace` when logs are not enough or when a process won’t respond.

---

## Lesson 4: pstack and gdb — Inspecting Stack Traces

Some systems include `pstack`, which prints a stack trace of a running process:

    sudo pstack 1234

If `pstack` is unavailable, use `gdb`:

    sudo gdb -p 1234

Inside gdb:
    bt

This is essential when:

- A multithreaded server is deadlocked  
- CPU is stuck at 100%  
- A C/C++ application is looping somewhere in user space  

---

## Lesson 5: Understanding Zombie and Defunct Processes

Zombie processes appear as `<defunct>` in `ps`:

List zombies:
    ps aux | grep defunct

A zombie process:

- Has completed execution  
- Has not been reaped by its parent  
- Consumes no CPU  
- Cannot be killed with `kill`  

Solution: fix or restart the **parent** process, not the zombie.

---

## Lesson 6: Debugging Hung Applications — Combining Tools

### Scenario 1: Application stuck and unresponsive

1. Identify CPU usage:
       ps aux --sort=-%cpu | head

2. See what files/sockets it's using:
       sudo lsof -p PID

3. Attach with strace:
       sudo strace -p PID

Look for:

- Permission denied errors  
- Repeated read/write loops  
- DNS or network timeouts  
- Missing configuration files  

### Scenario 2: Port in use and service fails to start

1. Check which process is bound to the port:
       sudo lsof -iTCP:8080 -sTCP:LISTEN

2. Stop or kill the offending process if appropriate.

### Scenario 3: Memory leak suspicion

1. Monitor process memory:
       ps -o pid,%mem,rss,cmd -p PID

2. Trace allocations (only possible with specialized tools or debug builds):
       sudo strace -p PID -e trace=memory

3. Inspect stack for growth or looping:
       sudo pstack PID
   or:
       sudo gdb -p PID
       bt

---

## Lab Exercise

Perform all exercises on a testing VM or sandbox.

1. Start a long-running process:
       sleep 1000 &

2. Use ps to find its PID:
       ps aux | grep sleep

3. Use lsof to inspect file handles:
       sudo lsof -p PID

4. Attach strace:
       sudo strace -p PID

Observe the repeating `nanosleep` calls.

5. Open a second terminal and run a process that opens a file:
       tail -f /var/log/syslog

6. Use lsof to identify which process has `/var/log/syslog` open:
       sudo lsof /var/log/syslog

7. If pstack is available:
       sudo pstack PID

Document all observations.

---

## Quiz

1. What information does `lsof` show that `ps` cannot?  
2. When would you use `strace -p PID` instead of checking application logs?  
3. How can you determine which process owns TCP port 80?  
4. What is a zombie process and how is it resolved?  
5. How would you diagnose a service startup failure when logs show nothing unusual?  

---

## Notes and Personal Observations

Use this space to document:

- Which tools are new  
- Any surprising system call patterns  
- Real application behavior you observed  
- Areas where you want more practice or deeper exploration

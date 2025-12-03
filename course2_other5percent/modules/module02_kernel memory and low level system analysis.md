# Module 02: Kernel, Memory, and Low-Level System Analysis (The Other 5%)

## Overview

This module covers low-level system diagnostics that cloud engineers and Linux administrators rarely need—but when they *do* need them, nothing else works. These commands help you diagnose kernel issues, memory leaks, OOM (Out-Of-Memory) conditions, runaway processes, kernel panics, and more.

You will learn how to:

- Inspect kernel logs  
- Analyze memory usage beyond `free -h`  
- Diagnose OOM kills  
- Understand slab allocation and kernel memory usage  
- Monitor kernel modules  
- Use `/proc` for deep system inspection  

Most of these tasks appear only during severe or hard-to-explain failures in production.

---

## Learning Objectives

By the end of this module, you should be able to:

- Read kernel-level logs using `dmesg` and journalctl  
- Identify OOM events and which processes were killed  
- Explore memory information through `/proc/meminfo`  
- Inspect kernel modules with `lsmod`, `modinfo`, and `modprobe`  
- Understand slab allocator statistics  
- Use `/proc` to diagnose low-level system problems  

---

## Lesson 1: Accessing Kernel Logs

System logs (`journalctl`) capture many events, but kernel logs are separate and often contain:

- Hardware errors  
- Disk I/O failures  
- Driver problems  
- OOM kills  
- Kernel warnings and panics  

View kernel messages:
    dmesg

Follow kernel messages in real time:
    dmesg -w

Filter for errors:
    dmesg | grep -i error

Filter for memory or OOM:
    dmesg | grep -i oom

`dmesg` output is essential when diagnosing resource exhaustion.

---

## Lesson 2: Using journalctl for Kernel-Level Insight

View only kernel logs since boot:
    journalctl -k

Follow kernel logs:
    journalctl -kf

View previous boot’s kernel logs:
    journalctl -k -b -1

This is useful after:

- A reboot due to crash  
- Kernel panic  
- Hardware issues  

---

## Lesson 3: Memory Analysis Beyond free -h

Check detailed memory info:
    cat /proc/meminfo

Key fields:

- `MemTotal`: Total RAM  
- `MemAvailable`: How much the kernel thinks is available  
- `Buffers` and `Cached`: File caching  
- `SwapTotal` and `SwapFree`  
- `Slab`: Kernel memory usage  

Check memory per process:
    ps -eo pid,ppid,cmd,%mem,rss | sort -k5 -n | tail

RSS (Resident Set Size) indicates how much RAM is actually in use.

Check swap activity:
    swapon --show

See what is using swap:
    grep VmSwap /proc/*/status 2>/dev/null

---

## Lesson 4: Diagnosing OOM (Out-Of-Memory) Events

The Linux kernel includes an OOM Killer that terminates processes when memory is exhausted.

Detect OOM events:
    dmesg | grep -i 'out of memory'

Or with journalctl:
    journalctl -k | grep -i oom

The logs will show:

- The process that triggered the OOM condition  
- The process that the kernel killed  
- Memory statistics at time of kill  

Check the killed process’s OOM score:
    cat /proc/PID/oom_score

Check its adjustment value:
    cat /proc/PID/oom_score_adj

Processes with higher scores are more likely to be killed.

---

## Lesson 5: Kernel Modules

Kernel modules are loaded drivers and optional components.

List all loaded modules:
    lsmod

Get information about a specific module:
    modinfo module_name

Load a module:
    sudo modprobe module_name

Unload a module:
    sudo modprobe -r module_name

In cloud engineering, you rarely manage modules manually unless dealing with:

- Custom AMIs  
- Specialized networking interfaces  
- Storage drivers  
- Kernel-level debugging  

---

## Lesson 6: Slab Allocator and Kernel Memory Usage

The kernel uses slab allocation for frequently used objects.

Inspect slab usage:
    cat /proc/slabinfo

Or summarized:
    slabtop

`slabtop` provides a live updating view of kernel memory allocations.

This is valuable when diagnosing:

- Kernel memory leaks  
- High slab usage  
- File descriptor exhaustion  
- Networking subsystem overload  

---

## Lesson 7: Deep System Inspection Using /proc

The `/proc` filesystem exposes internal kernel data.

See system uptime:
    cat /proc/uptime

CPU info:
    cat /proc/cpuinfo

Memory info:
    cat /proc/meminfo

System load averages:
    cat /proc/loadavg

Process information (example for PID 1234):
    cat /proc/1234/status
    cat /proc/1234/cmdline
    cat /proc/1234/limits
    ls  /proc/1234/fd

You can inspect:

- File descriptors  
- Environment variables  
- Limits  
- Current working directory  
- Threads  

Example: see all file descriptors:
    ls -l /proc/PID/fd

---

## Lesson 8: Diagnosing Kernel Panics, Hangs, and Hardware Issues

Kernel panics leave evidence in `dmesg` or `journalctl -k`.

Look for panics:
    dmesg | grep -i panic

Check hardware-related errors:
    dmesg | grep -i hardware
    dmesg | grep -i i/o

Check disk errors:
    dmesg | grep -i blk
    dmesg | grep -i sector

Check CPU or overheating warnings:
    dmesg | grep -i thermal

These logs are essential during severe outages.

---

## Lab Exercise

1. Review your system’s kernel logs:
       dmesg | head

2. Find disk-related warnings:
       dmesg | grep -i error

3. Examine `/proc/meminfo`:
       cat /proc/meminfo

4. View loaded kernel modules:
       lsmod

5. Identify slabs consuming the most memory:
       slabtop

6. Trigger an artificial heavy memory load (optional, only on test systems), then observe:

       journalctl -k | grep -i oom

Record results and observations.

---

## Quiz

1. What is the difference between `dmesg` and `journalctl -k`?  
2. How can you detect whether an OOM event occurred?  
3. What does `/proc/meminfo` tell you that `free -h` does not?  
4. When would you use `slabtop`?  
5. How do you list all loaded kernel modules?  

---

## Notes and Observations

Use this space to record:

- Unexpected kernel messages  
- Any slabs that looked unusually large  
- OOM scores you discovered  
- Open questions about memory or kernel logs  
- Areas you want to revisit or deepen

# Module 01: Introduction and Linux Mindset

## Overview

This module introduces what Linux is, why it matters for cloud engineering, and how to think about working on a Linux system. You will also run your first basic commands and get comfortable with the command-line interface (CLI).

The primary goal of this module is not to make you an expert, but to remove the fear of the terminal and give you a mental model of what you are looking at.

---

## Learning Objectives

By the end of this module, you should be able to:

- Explain what Linux is and why it is used so heavily in the cloud.  
- Understand the difference between the kernel, the shell, and user space.  
- Open a terminal and run basic commands without hesitation.  
- Use `whoami`, `pwd`, `ls`, and `man` to explore a system.  
- Use `--help` and manual pages to learn about new commands by yourself.

---

## Lesson 1: What Is Linux and Where Will You See It?

Linux is an operating system, just like Windows or macOS, but it is:

- Open-source  
- Highly configurable  
- Designed to be stable and multi-user  
- Dominant on servers and in the cloud

As a cloud engineer or cloud developer, you will interact with Linux in places like:

- Virtual machines (EC2, Compute Engine, etc.)  
- Container hosts (Docker, Kubernetes nodes)  
- CI/CD runners  
- Bastion/jump hosts  
- Database and application servers

You do not need to become a kernel developer. You do need to become comfortable operating, troubleshooting, and automating Linux-based systems.

---

## Lesson 2: The Linux Mental Model (Kernel, Shell, Userspace)

A simple model:

- **Hardware**: CPU, memory, disks, network cards.  
- **Kernel**: The core of the OS that talks to hardware and manages resources.  
- **User space**: All the programs you run (shell, editors, services, daemons).  
- **Shell**: The program that interprets commands you type (often `bash` or `sh`).

When you type commands in a terminal:

1. The shell reads your command.  
2. It finds the program (binary or script).  
3. It runs that program on top of the kernel.  
4. The kernel interacts with hardware as needed.

Understanding this helps later when you work with processes, system calls, logs, and performance.

---

## Lesson 3: Your First Commands

Open a terminal in your Linux environment and try the following.

### Who am I?

```bash
whoami

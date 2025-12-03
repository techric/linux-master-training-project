# Module 01: Introduction and Linux Mindset

## Overview

This module introduces what Linux is, why it matters for cloud engineering, and how to think about working on a Linux system. You will run your first basic commands and begin building the mental model required to navigate servers confidently.

The goal of this module is not mastery—it is orientation. You will remove the fear of the terminal and gain clarity about what Linux actually is and how the pieces fit together.

## Learning Objectives

By the end of this module, you should be able to:

- Explain what Linux is and why it is used heavily in cloud computing  
- Understand the difference between the kernel, the shell, and user space  
- Open a terminal and run basic commands  
- Use `whoami`, `pwd`, `ls`, and `man` to explore a system  
- Use `--help` and manual pages to learn about commands independently  

---

## Lesson 1: What Is Linux and Where Will You See It?

Linux is an operating system, similar in concept to Windows or macOS, but different in purpose. It is:

- Open-source  
- Designed for stability and performance  
- Multi-user and multi-process  
- Highly configurable  
- Dominant on servers and cloud platforms  

You will see Linux in:

- Virtual machines (EC2, Compute Engine, Azure VMs)  
- Kubernetes nodes  
- Docker containers  
- CI/CD runners  
- Bastion hosts and jump boxes  
- Database and application servers  

You do not need to become a kernel programmer. You need to become confident operating and troubleshooting Linux systems in real-world cloud environments.

---

## Lesson 2: The Linux Mental Model (Kernel, Shell, Userspace)

A simple conceptual model:

- **Hardware** — CPU, RAM, disks, network interface  
- **Kernel** — core of Linux; manages hardware and processes  
- **User space** — programs and tools you interact with (editors, utilities, services)  
- **Shell** — the program that interprets commands (bash, sh, zsh)

When you type a command:

1. The shell reads it  
2. The shell locates the executable  
3. The kernel runs it  
4. The kernel interacts with hardware as needed  

A clear mental model makes advanced topics like permissions, logs, system calls, and process troubleshooting easier later.

---

## Lesson 3: Your First Commands

### Who am I?

    whoami

Shows the username for the current session.

### Where am I?

    pwd

Displays your current working directory.

### List files in the current directory

    ls

More useful variations:

    ls -l
    ls -a
    ls -lah

### Learn about a command

    man ls

Or:

    ls --help

### Show recent command history

    history | tail

These simple commands give you visibility and control in the terminal.

---

## Lesson 4: Understanding Linux as a Multi-User System

Linux is designed so multiple users can operate on the same machine at the same time. This mindset shapes everything:

- Every file belongs to a user and a group  
- Every command runs under a specific user context  
- Permissions determine what you may or may not do  
- Scripts and services behave differently depending on which user runs them  

This is foundational for security, automation, and server operations.

---

## Lab Exercise

Perform the following steps:

1. Run these commands and record the output:

       whoami
       pwd

2. List all files in your current directory:

       ls -lah

3. Check which Linux distribution you're running:

       cat /etc/os-release

4. View your most recent commands:

       history | tail

5. Write down:

- Your username  
- Your home directory path  
- Your Linux distribution  
- Any hidden files you saw with `ls -a`  

---

## Quiz

Answer the following in your own words:

1. What role does the Linux kernel play?  
2. What does `pwd` show?  
3. What is the purpose of the shell?  
4. How can you learn more about a command?  
5. Why is Linux designed as a multi-user system?  

---

## Notes and Personal Observations

Use this section to record personal notes:

- What surprised you  
- What felt confusing  
- Commands you may want to revisit  
- Questions to return to later  

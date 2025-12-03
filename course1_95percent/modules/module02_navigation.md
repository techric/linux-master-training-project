# Module 02: Navigating the Linux Filesystem

## Overview

This module teaches you how to move around the Linux filesystem using the command-line interface (CLI). You will learn how Linux organizes files, how to change directories, how to inspect directory contents, and how to begin working with file paths.

Compared to Windows or macOS, Linux has a strict, standardized directory structure. Understanding this early will make all future Linux tasks easier, especially when working with servers, logs, or configuration files.

## Learning Objectives

By the end of this module, you should be able to:

- Explain how the Linux filesystem hierarchy is organized  
- Use `cd`, `ls`, and `pwd` to move around the system  
- Understand the difference between absolute and relative paths  
- Identify the purpose of key directories such as `/etc`, `/var`, `/home`, `/usr`, and `/opt`  
- Create and remove files and directories safely  
- Understand how to verify where you are on the filesystem at any time  

---

## Lesson 1: Understanding the Linux Filesystem Layout

Linux is organized differently from Windows. There are no drive letters such as `C:` or `D:`. Instead, everything begins at the root directory:

- `/` — the root of the entire filesystem

Key directories include:

- `/home` — user home directories  
- `/root` — home directory for the root (administrator) user  
- `/etc` — system configuration files  
- `/var` — variable data (logs, caches, temporary files, application data)  
- `/usr` — user-installed software, shared libraries, and system binaries  
- `/bin` and `/sbin` — essential system commands and system binaries  
- `/opt` — optional or third-party software  
- `/tmp` — temporary files (often cleared on reboot)

Understanding these locations will help you later when you need to find configuration files, logs, or application data.

---

## Lesson 2: Checking Your Current Location

To see where you are in the filesystem, use `pwd` (print working directory):

    pwd

Example output:

    /home/ec2-user

This tells you exactly which directory you are currently in.

---

## Lesson 3: Listing Files and Directories

Use `ls` to list files and directories.

Basic listing:

    ls

Common options:

- `ls -l` — long listing with details (permissions, owner, size, timestamps)  
- `ls -a` — show hidden files (those starting with a dot `.`)  
- `ls -lah` — long listing, all files, human-readable sizes

Examples:

    ls -l
    ls -a
    ls -lah

---

## Lesson 4: Moving Between Directories

Use `cd` (change directory) to move around.

Move into a directory:

    cd /etc

Move to your home directory:

    cd ~

Go up one level (to the parent directory):

    cd ..

Return to the previous directory:

    cd -

### Absolute vs. Relative Paths

- **Absolute path**: starts from `/` and describes the full path

      cd /var/log

- **Relative path**: based on your current directory

      cd logs

Relative paths are shorter but require you to know where you are (`pwd`).

---

## Lesson 5: Creating and Removing Files and Directories

Create a new directory:

    mkdir test_directory

Create an empty file:

    touch example.txt

List the directory to confirm:

    ls -l

Remove a file:

    rm example.txt

Remove an empty directory:

    rmdir test_directory

Remove a directory and everything inside it (use with extreme care):

    rm -r foldername

Always double-check:

    pwd

before using `rm -r`.

---

## Lab Exercise

Perform the following steps in your Linux environment.

1. Show your current directory:

       pwd

2. Navigate to `/etc`, list its contents, then return home:

       cd /etc
       ls -lah
       cd ~

3. Navigate to `/var`, list its contents, then return home:

       cd /var
       ls -lah
       cd ~

4. Create a practice directory under your home directory:

       mkdir ~/practice

5. Create a file inside the practice directory:

       touch ~/practice/test.txt

6. Move into the directory and confirm the file exists:

       cd ~/practice
       ls -l

7. Remove the file and then the directory:

       rm test.txt
       cd ..
       rmdir practice

Write down:

- Which directories you visited  
- Any directories you did not recognize  
- Any commands that did not behave as you expected  

---

## Quiz

Answer in your own words.

1. What is the root directory in Linux?  
2. What command shows your current working directory?  
3. What is the difference between an absolute path and a relative path?  
4. What extra information does `ls -lah` show compared to plain `ls`?  
5. Why should you be extremely careful when using `rm -r`?

---

## Notes and Personal Observations

Use this section to record your own notes:

- Paths you explored  
- Commands that you found useful  
- Any mistakes you made and how you fixed them  
- Questions you want to revisit later

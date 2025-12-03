# Module 06: Viewing and Understanding Linux System Logs

## Overview

Logs are one of the most important tools for troubleshooting Linux systems. Whether an application fails to start, a service crashes, or a server behaves unexpectedly, the first place you look is the logs.

Linux logs are typically stored under `/var/log` and managed by tools such as `rsyslog`, `journald`, and `systemd`. This module teaches you how to locate, view, search, and interpret logs.

## Learning Objectives

By the end of this module, you should be able to:

- Identify where Linux stores system and application logs  
- Use `cat`, `less`, `head`, and `tail` to read logs  
- Use `grep` to search logs for specific patterns  
- Use `journalctl` to read systemd-managed logs  
- Understand common log locations and what they contain  
- Troubleshoot basic system and service issues using logs  

---

## Lesson 1: Where Linux Stores Logs

Most Linux logs live under:

- `/var/log`

Common logs include:

- `/var/log/messages` — general system logs (RHEL/CentOS/Amazon Linux)  
- `/var/log/syslog` — general system logs (Debian/Ubuntu)  
- `/var/log/auth.log` — authentication events  
- `/var/log/secure` — security logs (RHEL-based)  
- `/var/log/dmesg` — kernel ring buffer  
- `/var/log/cloud-init.log` — cloud-init logging (cloud instances)  

Application logs may live in:

- `/var/log/<service>`  
- `/var/lib/<application>`  
- Custom directories defined by the application  

---

## Lesson 2: Viewing Logs with cat, less, head, and tail

### View an entire file (not ideal for large logs):

    cat /var/log/syslog

### View with scrolling:

    less /var/log/syslog

Inside `less`:

- `q` = quit  
- `/word` = search  
- `n` = next match  

### View the first 20 lines:

    head /var/log/syslog

### View the last 20 lines:

    tail /var/log/syslog

### Follow logs in real time (like watching live output):

    tail -f /var/log/syslog

or:

    tail -f /var/log/messages

This is extremely useful when debugging services.

---

## Lesson 3: Searching Logs with grep

`grep` lets you search logs for specific keywords or patterns.

Search for "error":

    grep "error" /var/log/syslog

Search case-insensitively:

    grep -i "failed" /var/log/auth.log

Show matching line numbers:

    grep -n "timeout" /var/log/messages

Search recursively in a directory:

    grep -R "nginx" /var/log

Combine with tail to search live output:

    tail -f /var/log/syslog | grep "ssh"

---

## Lesson 4: Using journalctl with systemd

If your system uses `systemd`, logs may be stored in the journal.

View all journal logs:

    sudo journalctl

View logs for a specific service:

    sudo journalctl -u sshd

View logs since last boot:

    sudo journalctl -b

View logs in real time:

    sudo journalctl -u nginx -f

View logs for the current user:

    journalctl --user

Limit by time:

    sudo journalctl --since "1 hour ago"
    sudo journalctl --since "2025-01-01" --until "2025-01-02"

---

## Lesson 5: Kernel Logs and dmesg

`dmesg` shows kernel messages.

View all messages:

    dmesg

Scroll through them:

    dmesg | less

View only recently added messages:

    dmesg --ctime | tail

Kernel logs help debug hardware issues, driver problems, boot errors, and container networking problems.

---

## Lesson 6: Understanding Common Log Patterns

Authentication failures:

    grep "Failed password" /var/log/auth.log

SSH logins:

    grep "Accepted password" /var/log/auth.log

Service crashes or restarts:

    sudo journalctl -u servicename -n 50

Network issues:

    grep "eth0" /var/log/messages

Disk or file system problems:

    grep -i "I/O error" /var/log/syslog

Cloud-init issues (cloud VMs):

    cat /var/log/cloud-init.log

Most real-world troubleshooting begins with identifying patterns like these.

---

## Lab Exercise

Complete the following tasks on your Linux system:

1. Open the general system log with `less`:

       less /var/log/syslog
       less /var/log/messages

2. Search for the word "error":

       grep -i "error" /var/log/syslog

3. Watch authentication events in real time:

       tail -f /var/log/auth.log

4. View the last 50 log entries for a service:

       sudo journalctl -u sshd -n 50

5. View logs since last boot:

       sudo journalctl -b

6. Run:

       dmesg | tail

Record anything unexpected.

---

## Quiz

1. Where are most Linux logs stored?  
2. What is the difference between `tail` and `tail -f`?  
3. What command searches logs for specific text?  
4. What log viewer is used for systemd services?  
5. What type of information does `dmesg` show?  

---

## Notes and Personal Observations

Use this section to record:

- Patterns you observed  
- Errors you found in logs  
- Commands that were useful  
- Topics you want to revisit  

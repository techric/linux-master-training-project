# Module 07: Monitoring System Resources and Performance

## Overview

This module teaches you how to check CPU usage, memory usage, disk usage, running processes, open network ports, and overall system performance. These monitoring commands are essential for diagnosing slow servers, failed deployments, unresponsive applications, and resource exhaustion.

As a cloud engineer, you will use these tools constantly when troubleshooting.

## Learning Objectives

By the end of this module, you should be able to:

- View active processes and resource usage  
- Check CPU, memory, and load average  
- Monitor disk usage and disk space  
- Identify large directories and files  
- Check open network connections and listening ports  
- Use real-time monitoring tools such as `top` and `htop`  
- Understand how to troubleshoot high CPU or RAM usage  

---

## Lesson 1: Checking Running Processes

Use `ps` to view running processes.

Show all processes:

    ps aux

Search for a specific process:

    ps aux | grep nginx

View processes for the current user:

    ps -u $USER

---

## Lesson 2: Real-Time Resource Monitoring (top / htop)

### top (installed by default)

Start it:

    top

Inside `top`:

- `q` = quit  
- `P` = sort by CPU  
- `M` = sort by memory  

### htop (if installed)

    htop

`htop` provides a more readable, colorized interface.

Install it:

    sudo apt install htop
    sudo yum install htop
    sudo dnf install htop

---

## Lesson 3: CPU and Load Average

Display system load and uptime:

    uptime

Example output:

    17:22:10 up 10 days,  4:03,  load average: 0.12, 0.25, 0.18

Load average represents CPU demand over 1, 5, and 15 minutes.

View detailed CPU info:

    cat /proc/cpuinfo

Monitor CPU usage:

    top
    htop

---

## Lesson 4: Memory Usage

Check memory and swap usage:

    free -h

Example output shows:

- Total memory  
- Used memory  
- Available memory  
- Swap usage  

View memory per process in `top` or `htop`.

---

## Lesson 5: Disk Space and Disk Usage

Check overall disk space:

    df -h

Check usage of a specific directory:

    du -sh /var/log

List largest directories:

    du -sh * | sort -rh | head

List largest files:

    find / -type f -size +100M

Disk issues are common root causes of crashes or failed writes.

---

## Lesson 6: Network Connections and Open Ports

List open listening ports:

    sudo ss -tulnp

Breakdown:

- `t` = TCP  
- `u` = UDP  
- `l` = listening  
- `n` = numeric output  
- `p` = show process  

View network interface details:

    ip a

View routing table:

    ip route

Ping a host:

    ping -c 4 google.com

Check DNS resolution:

    dig google.com
    nslookup google.com

---

## Lesson 7: Monitoring Logs in Real Time

Combine monitoring with logging:

    tail -f /var/log/syslog
    tail -f /var/log/messages

Watch logs while restarting a service:

    sudo systemctl restart nginx
    sudo journalctl -u nginx -f

---

## Lesson 8: Troubleshooting High Resource Usage

### High CPU

Find top consumers:

    top
    ps aux --sort=-%cpu | head

Kill a runaway process:

    sudo kill -9 <PID>

### High Memory

Identify processes consuming memory:

    ps aux --sort=-%mem | head

Check swap usage:

    free -h

### Low Disk Space

Find large directories:

    du -sh * | sort -rh | head

Clean logs:

    sudo journalctl --vacuum-size=100M

Clean package caches:

    sudo apt clean
    sudo dnf clean all
    sudo yum clean all

---

## Lab Exercise

1. View all running processes:

       ps aux

2. Launch `top` and sort by CPU:

       top

3. Check memory usage:

       free -h

4. Check disk space:

       df -h

5. List the 10 largest directories in `/var`:

       du -sh /var/* | sort -rh | head

6. View all open ports:

       sudo ss -tulnp

7. View CPU information:

       cat /proc/cpuinfo

Record any unexpected results.

---

## Quiz

1. What command shows all running processes?  
2. What does `df -h` display?  
3. What does the load average represent?  
4. How do you see which process is using the most CPU?  
5. What command lists all open listening network ports?  

---

## Notes and Personal Observations

Use this space to record:

- Resource usage patterns you observed  
- Commands you found especially useful  
- Any anomalies you noticed in your system  

# Module 04: Managing Services and System Processes with systemd

## Overview

This module introduces `systemd`, the service manager used on most modern Linux distributions. As a cloud engineer, you must understand how to start, stop, enable, disable, and check the status of services. Many common troubleshooting tasks—such as failing web servers, crashed applications, or slow systems—come down to service or process issues.

This module provides the foundational commands for checking and controlling system services.

## Learning Objectives

By the end of this module, you should be able to:

- Understand what `systemd` is and why it is used  
- List running services and inspect their status  
- Start, stop, restart, enable, and disable services  
- Read system logs using `journalctl`  
- Understand how services relate to processes and daemons  
- Troubleshoot common service failures  

---

## Lesson 1: What Is systemd?

`systemd` is the default service and init system on most major Linux distributions, including:

- Ubuntu  
- Debian  
- Amazon Linux 2 / 2023  
- CentOS / RHEL  
- Fedora  

`systemd` controls:

- Service startup and shutdown  
- Logging  
- Process supervision  
- Boot targets (runlevels)  
- Resource management  

In real cloud environments, you will work with `systemd` constantly.

---

## Lesson 2: Checking the Status of a Service

To check the status of a service:

    systemctl status sshd

To check all active services:

    systemctl list-units --type=service

To check all services (even stopped ones):

    systemctl list-unit-files --type=service

This helps identify what is running, what failed, and what has recently crashed.

---

## Lesson 3: Starting, Stopping, and Restarting Services

Start a service:

    sudo systemctl start nginx

Stop a service:

    sudo systemctl stop nginx

Restart a service:

    sudo systemctl restart nginx

Reload configuration without restarting the process:

    sudo systemctl reload nginx

Common when you modify configuration files (e.g., `/etc/nginx/nginx.conf`).

---

## Lesson 4: Enabling and Disabling Services at Boot

Enable a service to start automatically at boot:

    sudo systemctl enable nginx

Disable a service:

    sudo systemctl disable nginx

View whether a service is enabled:

    systemctl is-enabled nginx

Cloud engineers frequently work with services that must survive reboots, especially web servers and monitoring agents.

---

## Lesson 5: Viewing Logs with journalctl

`journalctl` lets you view logs collected by `systemd-journald`.

View logs for a specific service:

    sudo journalctl -u nginx

View logs in real time (like `tail -f`):

    sudo journalctl -u nginx -f

View logs since the last boot:

    sudo journalctl -b

View only the last 100 lines:

    sudo journalctl -u sshd -n 100

View logs with timestamps:

    sudo journalctl --since "2 hours ago"

Logs are essential for troubleshooting failed services.

---

## Lesson 6: Understanding Units, Daemons, and Processes

A **unit** is a resource `systemd` manages.  
A **daemon** is a background service, often ending in `d` (e.g., `sshd`, `crond`).  
A **process** is an instance of a running program.

Useful command for checking processes:

    ps aux

And to search for a specific process:

    ps aux | grep nginx

To get real-time resource usage:

    top

or its more user-friendly version:

    htop

(If installed)

---

## Lesson 7: Common Service Troubleshooting Patterns

### Service is active but not working

Check logs:

    sudo journalctl -u servicename

Check ports:

    sudo ss -tulnp

### Service fails to start

Look at the last few logs:

    sudo journalctl -u servicename -n 50

Check its configuration file for syntax errors.

### Service starts but immediately stops

Common causes:

- Wrong file permissions  
- Bad configuration syntax  
- Missing directories or files  
- Port already in use  

### Service works until reboot, then fails

Often caused by:

- Service not enabled  
- Dependency services missing  
- Network not ready at boot  

---

## Lab Exercise

Complete the following:

1. Check the status of the SSH daemon:

       systemctl status sshd

2. Pick a service installed on your system (e.g., `cron`, `sshd`, `nginx`) and restart it:

       sudo systemctl restart <service>

3. View the last 50 lines of logs for that service:

       sudo journalctl -u <service> -n 50

4. Check all enabled services:

       systemctl list-unit-files --type=service | grep enabled

5. Disable a service (pick something safe like `bluetooth` if available):

       sudo systemctl disable <service>

6. Re-enable the service:

       sudo systemctl enable <service>

7. Verify with:

       systemctl is-enabled <service>

Record anything unusual or unexpected.

---

## Quiz

1. What is `systemd` and what does it manage?  
2. What command shows the status of a service?  
3. What is the difference between `restart` and `reload`?  
4. How do you enable a service to start at boot?  
5. How do you view logs for a service in real time?  

---

## Notes and Personal Observations

Record:

- Services you tested  
- Logs you examined  
- Any errors you encountered  
- Concepts that need more review  

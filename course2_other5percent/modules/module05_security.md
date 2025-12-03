# File: course2_other5percent/modules/module05_security.md

# Module 05: Advanced Security, Hardening, and Intrusion Diagnostics  
(The Other 5%)

## Overview

This module covers advanced Linux security inspection and hardening techniques used when you must diagnose suspicious activity, investigate unauthorized access, lock down a production system, or ensure compliance.

These topics appear far less frequently in everyday cloud engineering work—but when they do appear, they are critical. This is the “last line of defense” skill set.

Topics include:

- System auditing  
- Privilege escalation tracing  
- SELinux/AppArmor diagnostics  
- Investigating unauthorized access  
- Detecting malicious processes and persistence mechanisms  
- Hardening SSH and system users  
- Kernel-level security controls  

---

## Learning Objectives

By the end of this module, you should be able to:

- Inspect login history and authentication failures  
- Use auditd to capture system-level security events  
- Identify suspicious processes and persistence mechanisms  
- Diagnose SELinux/AppArmor denials  
- Audit system users, groups, and permissions  
- Harden SSH configuration and remove weak settings  
- Understand kernel security modules and capabilities  

---

## Lesson 1: Auditing Logins, Failures, and Sudo Use

Check login history:
    last

Check failed login attempts:
    lastb

View recent authentication logs:
    sudo tail -n 50 /var/log/auth.log      # Debian/Ubuntu
    sudo tail -n 50 /var/log/secure        # RHEL/CentOS/Amazon

List all sudo attempts:
    sudo cat /var/log/auth.log | grep sudo

Identify brute-force attempts by IP:
    sudo grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr

---

## Lesson 2: System Audit Framework (auditd)

Auditd records low-level security events such as file modifications, policy changes, privilege escalations, and more.

Install auditd:
    sudo apt install auditd
    sudo systemctl enable --now auditd

View recent audit logs:
    sudo ausearch -m USER_LOGIN
    sudo ausearch -m USER_CMD

Search for suspicious privilege escalation:
    sudo ausearch -m USER_CMD | grep -i "chmod"

Monitor specific files:
    sudo auditctl -w /etc/passwd -p wa -k passwd_changes

Search by key:
    sudo ausearch -k passwd_changes

Audit logs become essential during post-incident analysis.

---

## Lesson 3: Advanced Permissions, Capabilities, and SetUID

List all setuid binaries (high-privilege targets):
    sudo find / -perm -4000 -type f 2>/dev/null

List Linux capabilities:
    sudo getcap -r /usr/bin

Capabilities allow partial privileges without full root access.

Check running process capabilities:
    cat /proc/PID/status | grep Cap

If an attacker elevates privileges, these areas often reveal traces.

---

## Lesson 4: SELinux and AppArmor Diagnostics

### SELinux

Check SELinux mode:
    getenforce

View SELinux denials:
    sudo ausearch -m avc

Extended logs:
    sudo sealert -a /var/log/audit/audit.log

Temporarily set permissive mode (for debugging only):
    sudo setenforce 0

Return to enforcing:
    sudo setenforce 1

### AppArmor

Check loaded profiles:
    sudo aa-status

View violations:
    sudo journalctl -xe | grep DENIED

AppArmor and SELinux explain many “works on Ubuntu, broken on Amazon Linux” issues.

---

## Lesson 5: Investigating Suspicious Processes and Persistence

List processes with unusual parents:
    ps -eo pid,ppid,cmd --sort=ppid

View network connections:
    sudo ss -tulpn

List processes listening externally:
    sudo ss -ltnp | grep 0.0.0.0

List open sockets:
    sudo lsof -i

Find hidden processes:
    sudo ls /proc | grep -E '^[0-9]+$' | wc -l

Compare with:
    ps aux | wc -l

If numbers differ significantly, malware may be hiding.

### Persistence vectors to audit

Cron jobs:
    sudo crontab -l
    sudo ls -l /etc/cron.*

Systemd autostart:
    sudo systemctl list-unit-files --type=service

Check for suspicious new services:
    sudo systemctl status service_name

User startup scripts (common malware location):
    ~/.bashrc  
    ~/.bash_profile  
    ~/.config/autostart  

Search for reverse shell patterns:
    sudo grep -R "nc -e" /
    sudo grep -R "bash -i" /

---

## Lesson 6: SSH Hardening and Audit Techniques

Inspect SSH configuration:
    sudo cat /etc/ssh/sshd_config

Important items:

Disable password authentication (if allowed):
    PasswordAuthentication no

Disable root login:
    PermitRootLogin no

Limit users:
    AllowUsers youruser

Restrict ciphers:
    Ciphers aes256-ctr

Restart SSH:
    sudo systemctl restart sshd

View SSH attempts:
    sudo grep "sshd" /var/log/auth.log

Identify IP addresses performing brute force:
    sudo grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr

---

## Lesson 7: Kernel Security Controls

Check kernel parameters:
    sudo sysctl -a

Useful parameters:

Enable ASLR (address randomization):
    sudo sysctl -w kernel.randomize_va_space=2

Restrict core dumps:
    sudo sysctl -w fs.suid_dumpable=0

Restrict ptrace debugging:
    sudo sysctl -w kernel.yama.ptrace_scope=1

Persist settings in:
    /etc/sysctl.conf

These are rarely used daily but matter for hardening production servers.

---

## Lesson 8: Real-World Scenarios

### Scenario 1: Unauthorized login detected

1. Check timestamps:
       last | head

2. Check failure logs:
       lastb | head

3. Check ssh logs:
       sudo grep sshd /var/log/auth.log

4. Check for new systemd services:
       sudo systemctl list-unit-files --type=service

5. Check cron jobs:
       sudo crontab -l

---

### Scenario 2: Application cannot run due to SELinux

View denial:
       sudo ausearch -m avc

Interpret the rule:
       sealert -a /var/log/audit/audit.log

Fix by modifying SELinux policy:
       sudo semanage fcontext -a -t httpd_sys_content_t "/app(/.*)?"
       sudo restorecon -R /app

---

### Scenario 3: Suspected malware process

1. Identify new processes:
       ps aux --sort=start_time | tail

2. Identify unexpected open ports:
       sudo ss -tulpn

3. Check persistence (cron/systemd):
       sudo systemctl list-unit-files --type=service
       sudo crontab -l

4. Dump process memory (advanced):
       sudo gcore PID

5. Kill and quarantine the process:
       sudo kill -9 PID

---

## Lab Exercise

1. Display all failed login attempts:
       lastb

2. Identify all processes with open TCP ports:
       sudo ss -tulpn

3. Audit /etc/shadow for writes:
       sudo auditctl -w /etc/shadow -p wa -k shadow_changes
       sudo ausearch -k shadow_changes

4. List all setuid binaries:
       sudo find / -perm -4000 -type f 2>/dev/null

5. Trigger an SELinux denial (optional; testing VM only).

Document findings.

---

## Quiz

1. What does auditd monitor that journald does not?  
2. What is a common sign of privilege escalation in logs?  
3. How do SELinux denials typically appear in audit logs?  
4. Why should setuid binaries be monitored closely?  
5. What SSH settings reduce brute-force attack success?  

---

## Notes and Observations

Use this section to record:

- Suspicious logs you found  
- Unexpected processes or open ports  
- Any SELinux/AppArmor conflicts  
- Hardening changes you plan to apply in the future  

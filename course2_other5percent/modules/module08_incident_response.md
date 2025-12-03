# Module 08: Linux Incident Response, Forensics, and Post-Mortem Analysis  
(The Other 5%)

## Overview

This module covers advanced, high-stakes skills used only when something has gone seriously wrong. These are the techniques used when responding to:

- Security breaches or suspected intrusions  
- Data corruption or unexpected deletion  
- Ransomware or cryptomining infections  
- Malware persistence or privilege escalation  
- System compromise via SSH or exposed services  
- Insider threats  
- Production outages with unknown cause  
- Crashes that leave partial or incomplete logs  

These topics form the backbone of Linux incident response in both enterprise and cloud environments.

---

## Learning Objectives

By the end of this module, you should be able to:

- Secure and isolate a potentially compromised system  
- Collect volatile evidence (processes, network connections, memory)  
- Identify persistence mechanisms and malicious binaries  
- Analyze system logs for suspicious activity  
- Detect unauthorized privilege escalation  
- Perform timeline analysis using filesystem metadata  
- Extract forensic data without corrupting the system  
- Document findings for a full post-mortem  

---

## Lesson 1: Initial Triage and System Isolation

Freeze the system state immediately:

Show logged-in users:
    who

Show all sessions:
    w

List all running processes:
    ps aux

List network connections:
    sudo ss -tulpn

Disable external access WITHOUT killing the system:
    sudo iptables -P INPUT DROP
    sudo iptables -P OUTPUT DROP

Or on cloud platforms (AWS, GCP, Azure):

- Remove public IP  
- Apply deny-all network ACL or security group  
- Snapshot disks

Isolation prevents attackers from continuing activity or destroying logs.

---

## Lesson 2: Collecting Volatile Evidence (Before Reboot)

Show processes with parents:
    ps -eo pid,ppid,cmd:80 --sort=ppid

Dump memory (if gcore available):
    sudo gcore -o /root/memdump PID

View kernel ring buffer:
    dmesg | tail -n 50

Show open files:
    sudo lsof

Take a full memory dump (advanced / requires tools):
    sudo makedumpfile /proc/kcore /root/dumpfile

Or using LiME (Linux Memory Extractor) in advanced forensic work.

Volatile evidence disappears after reboot—capture it early.

---

## Lesson 3: Identifying Suspicious Processes and Binaries

List processes without matching executable on disk:
    sudo ls -l /proc/*/exe 2>/dev/null | grep deleted

These often indicate:

- Cryptominers  
- Malware injected into memory  
- Temporary reverse shells  

List unexpected binaries in PATH directories:
    sudo find /usr/local/bin -type f -exec ls -l {} \;

Search for binaries modified recently:
    sudo find / -type f -mtime -2 -exec ls -l {} \;

Search for files with abnormal permissions:
    sudo find / -perm -4000 -type f

Attackers often modify or drop binaries in writable directories.

---

## Lesson 4: Persistence Identification (The Most Important Step)

Attackers attempt persistence to survive reboots.

### Common persistence vectors:

Systemd services:
    sudo systemctl list-unit-files --type=service

Cron jobs:
    sudo crontab -l
    sudo ls /etc/cron.*

User login scripts:
    cat ~/.bashrc
    cat ~/.bash_profile

SSH authorized keys:
    cat ~/.ssh/authorized_keys

Unexpected timers:
    sudo systemctl list-timers

Kernel modules:
    lsmod

Any unexpected entries require investigation.

---

## Lesson 5: Log Analysis for Incident Response

View recent authentication activity:
    sudo egrep "Failed|Accepted" /var/log/auth.log

Search for new users:
    sudo grep "useradd" /var/log/auth.log

Search for privilege escalation attempts:
    sudo grep "sudo" /var/log/auth.log

Identify suspicious SSH activity:
    sudo grep "sshd" /var/log/auth.log

Extract logs for a specific timeframe:
    journalctl --since "2025-01-30" --until "2025-01-31"

Correlate across:

- Authentication logs  
- Kernel logs  
- Application logs  
- Network service logs  

A timeline quickly starts to emerge.

---

## Lesson 6: Filesystem Timeline Analysis

Show files modified in the last hour:
    sudo find / -mmin -60 -exec ls -l {} \;

Show new files:
    sudo find / -ctime -1 -exec ls -l {} \;

Show recently changed permissions:
    sudo find / -cmin -30 -exec ls -l {} \;

Check for unauthorized sudoers changes:
    sudo cat /etc/sudoers
    sudo ls -l /etc/sudoers.d/

Timeline analysis helps reconstruct:

- Entry vector  
- Attacker behavior  
- Persistence  
- Data exfiltration  
- Effects on system files  

---

## Lesson 7: Malware & Compromise Indicators

### Network Indicators

Unexpected outbound traffic:
    sudo ss -tupn | grep ESTAB

Connections to foreign IPs:
    sudo ss -tupn | grep -v 127.0.0.1

### Process Indicators

High CPU load by unknown processes:
    top

Hidden processes:
    sudo ls /proc | grep -E '^[0-9]+$' | wc -l
    ps aux | wc -l

### File Indicators

Binaries missing from disk but present in memory:
    ls -l /proc/*/exe | grep deleted

Suspicious systemd unit:
    sudo systemctl status suspicious.service

Unexpected SSH keys in accounts:
    cat ~/.ssh/

---

## Lesson 8: Evidence Preservation and Disk Forensics

Create a full block device image (when allowed):
    sudo dd if=/dev/sda of=/root/disk.img bs=4M status=progress

Mount read-only for analysis:
    sudo mount -o ro,loop disk.img /mnt/forensics

Checksum evidence:
    sha256sum /root/disk.img

Store evidence externally before further action.

---

## Lesson 9: Recovery, Remediation, and Hardening

Steps usually taken:

1. Identify point of entry  
2. Identify attacker actions  
3. Remove persistence mechanisms  
4. Patch vulnerabilities  
5. Rotate credentials  
6. Rebuild instances from clean AMIs or golden images  
7. Apply hardening baseline  

Cloud environments often use:

- Auto-scaling groups  
- Immutable infrastructure  
- CI/CD pipelines  
- Rapid redeploy instead of manual cleanup  

---

## Lesson 10: Documentation and Post-Mortem

Document everything:

Incident timeline:
    Events → Detection → Containment → Eradication → Recovery

Evidence:
- Logs  
- Screenshots  
- Command outputs  
- Disk images  

Root cause:
- Vulnerability exploited  
- Misconfiguration  
- Outdated package  

Preventive recommendations:
- IAM hardening  
- Patching schedule  
- Secure SSH baseline  
- Monitoring alerts  

---

## Lab Exercise

1. Simulate suspicious login activity:
       sudo tail -f /var/log/auth.log

2. Identify running processes without binaries:
       ls -l /proc/*/exe | grep deleted

3. Create a mock persistence service (in a VM only):
       sudo nano /etc/systemd/system/persist.service
       sudo systemctl enable persist.service

4. Trace all SSH activity for the last hour:
       journalctl -u ssh --since "-1 hour"

Document your findings.

---

## Quiz

1. Why should volatile memory be collected before rebooting?  
2. Name three common persistence mechanisms attackers use.  
3. What command shows processes with deleted binaries?  
4. Why is timeline analysis critical for incident response?  
5. What is the purpose of using dd for disk imaging?  

---

## Notes and Observations

Record:

- Indicators of compromise you observed  
- Logs that hinted at attacker behavior  
- Persistence mechanisms you found  
- What you would improve in a real-world IR process  

# Capstone Project: Full-System Incident Response, Forensics, and Recovery Simulation  
(The Other 5%)

## Overview

This capstone simulates a real-world Linux compromise scenario—something a senior cloud engineer or Linux administrator might encounter during an emergency escalation. The goal is to demonstrate mastery of all advanced diagnostic, forensic, and recovery techniques from the “Other 5%” modules.

You will be placed into a broken, suspicious, or partially compromised environment and must respond as if this were a real production incident.

This project evaluates your ability to:

- Perform rapid triage  
- Preserve evidence  
- Trace intrusion vectors  
- Identify malicious activity  
- Develop a timeline of the attack  
- Remove malware and persistence  
- Restore system integrity  
- Document findings in a professional post-mortem  

The expectation is senior-level thinking, process, and rigor.

---

# Scenario Background

You are the on-call Linux/cloud engineer for a mid-size SaaS company. At 03:14 AM, monitoring alerts indicate:

- CPU spiking to 100% for 15 minutes  
- 2 failed deployments to production  
- A sudden outbound network connection to an IP in a foreign country  
- Application logs suddenly stopped writing  
- Users reporting intermittent outages  

The system is a Linux-based application server running in the cloud (AWS/GCP/Azure).

You open the console and discover:

- The filesystem is partly read-only  
- A mysterious systemd service has appeared  
- A process exists in memory but no longer exists on disk  
- Authentication logs show multiple failed login attempts from suspicious IPs  
- The server cannot be rebooted because it is currently handling partial production traffic  

Your task is to investigate, preserve, recover, and document.

---

# Requirements

You must complete **ALL** of the following tasks to pass the capstone:

---

## Task 1: System Isolation

Isolate the machine without killing active processes.

1. Drop outbound and inbound access:
       sudo iptables -P INPUT DROP
       sudo iptables -P OUTPUT DROP

2. Disable public access using cloud security groups or firewalls.

3. Capture:
- The state of logged-in users  
- Running processes  
- Network connections  
- SSH sessions  
- Background services  

Commands to include in documentation:
       who
       w
       ps aux
       sudo ss -tulpn
       journalctl -u ssh

---

## Task 2: Collect Volatile Memory Evidence

Collect all volatile system information BEFORE any reboot.

Evidence includes:

- Process listing with parents:
       ps -eo pid,ppid,user,cmd --sort=ppid

- Open files:
       sudo lsof

- Deleted binaries still running:
       ls -l /proc/*/exe | grep deleted

- Kernel issues:
       dmesg | tail -n 50

- Memory dump (if available):
       sudo gcore -o /root/memdump PID

You must save ALL output.

---

## Task 3: Identify Initial Indicators of Compromise (IOCs)

Search for:

- Suspicious systemd services:
       sudo systemctl list-unit-files --type=service

- Cron jobs:
       sudo crontab -l
       sudo ls /etc/cron.*

- Unauthorized SSH keys:
       cat ~/.ssh/authorized_keys

- Unexpected network connections:
       sudo ss -tupn

List all IOCs you discover.

---

## Task 4: Log Analysis and Intrusion Timeline

Using journald, auth logs, and kernel logs:

1. Extract key authentication events:
       sudo egrep "Failed|Accepted" /var/log/auth.log

2. Identify brute-force attempts:
       sudo grep "Failed password" /var/log/auth.log

3. Trace attacker login success:
       journalctl --since "2 hours ago" -u ssh

4. Extract kernel logs for filesystem or memory issues:
       dmesg | grep -i error

Create a full timeline:

- First suspicious activity  
- First failed login  
- First successful login  
- Persistence installation  
- Malware execution  
- Outbound connection initiated  

---

## Task 5: Filesystem Analysis and Persistence Removal

Search for modified, suspicious, or newly created files:

       sudo find / -mmin -60 -exec ls -l {} \;

Identify persistence mechanisms:

- systemd services  
- cron jobs  
- SSH keys  
- modified bash profiles  
- unexpected binaries in /usr/local/bin  

Once identified, document EXACT paths and provide removal steps.

---

## Task 6: Reverse Engineering the Malicious Process (Lightweight)

Without advanced tooling, analyze the suspicious running process.

1. Identify its binary name (from deleted exe path)
2. Extract strings:
       strings /proc/PID/exe | head -n 50

3. Identify:
- Network patterns  
- File access patterns  
- Hardcoded IPs  
- Suspicious strings  

Document all observations.

---

## Task 7: Perform a Safe Disk Image Backup

Create a forensic disk image WITHOUT altering the system:

       sudo dd if=/dev/sda of=/root/forensic.img bs=4M status=progress

Then checksum:

       sha256sum /root/forensic.img

Include the checksum in documentation.

---

## Task 8: Full System Recovery Plan

Once evidence is captured, propose a recovery plan:

- Remove persistence  
- Disable unauthorized accounts  
- Remove compromised keys  
- Patch packages  
- Update kernel  
- Rebuild damaged system services  
- Return filesystem to read-write (after fsck in a rescue VM)  
- Validate application integrity  
- Restore logging  
- Re-enable network traffic  
- Reintroduce the instance to the load balancer  

This must be written as a step-by-step playbook.

---

## Task 9: Final Post-Mortem Report

Your post-mortem must include:

1. Executive summary  
2. Scope of impact  
3. Timeline of events  
4. Root cause analysis  
5. Detection/alert gaps  
6. Infrastructure weaknesses  
7. Recommendations for prevention  
8. Hardening checklist  

Format this report as if it were going to:

- CTO  
- Security team  
- Cloud engineering leadership  

---

# Deliverables

You must produce:

- A complete incident timeline  
- A list of ALL indicators of compromise  
- A description of the root cause  
- All evidence collected (ps, ss, lsof, dmesg, journalctl, etc.)  
- A remediation playbook  
- A full post-mortem document  

The deliverables must be added into your GitHub repository under:

course2_other5percent/capstone/

---

# Success Criteria

You pass the capstone if:

- You follow proper forensic procedure  
- You isolate the system correctly  
- You gather all volatile evidence  
- You identify ALL persistence mechanisms  
- Your timeline is accurate  
- You produce a professional post-mortem  
- Your remediation plan is complete and technically sound  

If any step is skipped or corrupted, assume failure in a real incident.

---

# End of Capstone

# Capstone Project: Real-World Linux Troubleshooting and Operations

## Overview

This capstone project brings together everything you learned in Modules 01–09. You will perform real-world Linux tasks involving navigation, permissions, service management, logging, networking, monitoring, and shell scripting.

This assignment simulates what a junior cloud engineer or Linux administrator would face during onboarding or in a technical interview.

## Objective

By the end of this capstone, you should be able to:

- Navigate a Linux system confidently  
- Manage files, permissions, and directories  
- Inspect and control systemd services  
- Analyze logs to troubleshoot failures  
- Verify networking and connectivity  
- Monitor CPU, memory, disk, and processes  
- Write basic scripts to automate tasks  
- Document your work in a structured way  

---

# Scenario

You have been hired as a Junior Cloud Engineer. You are given access to a new Linux server that another team reported as “slow,” “inconsistent,” and “throwing errors.”

Your job is to:

1. Investigate  
2. Troubleshoot  
3. Repair  
4. Document  

Use **only standard Linux tools** covered in this course.

---

# Tasks

## Task 1: System Orientation and User Information

1. Display:
       whoami
       hostname
       pwd

2. List all files in your home directory:
       ls -lah ~

3. Document:
- Your username  
- Your home directory  
- The Linux distribution (`cat /etc/os-release`)  

---

## Task 2: Directory and File Operations

1. Create a project directory:

       mkdir ~/capstone_project

2. Inside it, create the following structure:

       logs/
       scripts/
       reports/

3. Create an empty log file:

       touch ~/capstone_project/logs/app.log

4. Create a text file with system information:

       echo "Capstone Report" > ~/capstone_project/reports/system_info.txt
       date >> ~/capstone_project/reports/system_info.txt
       uname -a >> ~/capstone_project/reports/system_info.txt

---

## Task 3: Permissions and Ownership

1. Create a new directory:

       mkdir ~/capstone_project/secure_data

2. Make it accessible **only to you**:

       chmod 700 ~/capstone_project/secure_data

3. Create a file inside it:

       touch ~/capstone_project/secure_data/secret.txt

4. Verify permissions:

       ls -l ~/capstone_project/secure_data

---

## Task 4: Service Management with systemd

1. Check the status of the SSH service:

       systemctl status sshd

2. Restart the service:

       sudo systemctl restart sshd

3. Check logs for the last 20 entries:

       sudo journalctl -u sshd -n 20

4. Document:
- Is the service running?  
- Any unusual errors?  

---

## Task 5: Log Investigation

1. View the last 50 lines of the system log:

       sudo tail -n 50 /var/log/syslog
       sudo tail -n 50 /var/log/messages

2. Search for “error,” “failed,” or “timeout”:

       grep -i "error" /var/log/syslog
       grep -i "failed" /var/log/auth.log

3. Document:
- Any suspicious entries  
- Timestamp of most recent error  

---

## Task 6: Monitoring and Resource Usage

1. Check memory usage:

       free -h

2. Check disk space:

       df -h

3. List top 10 CPU-consuming processes:

       ps aux --sort=-%cpu | head

4. List top 10 memory-consuming processes:

       ps aux --sort=-%mem | head

5. Document:
- CPU pressure  
- Memory usage  
- Disk space concerns  

---

## Task 7: Networking and Connectivity

1. Show network interfaces:

       ip a

2. Show routing table:

       ip route

3. Test DNS and external connectivity:

       ping -c 3 8.8.8.8
       dig google.com
       curl -I https://www.google.com

4. Check open listening ports:

       sudo ss -tulnp

5. Document:
- Whether the server can reach the internet  
- Whether DNS is working  
- Which ports are listening  

---

## Task 8: Shell Script Automation

Create a script named `healthcheck.sh` inside `~/capstone_project/scripts/`:

       nano ~/capstone_project/scripts/healthcheck.sh

The script must:

1. Print the system date  
2. Print CPU load (`uptime`)  
3. Print memory usage (`free -h`)  
4. Print disk usage (`df -h`)  
5. Print the top 5 CPU-consuming processes  
6. Append all results to `~/capstone_project/reports/health_report.txt`

Make the script executable:

       chmod +x ~/capstone_project/scripts/healthcheck.sh

Run it:

       ./capstone_project/scripts/healthcheck.sh

---

## Task 9: Final Report

Create a final report here:

       ~/capstone_project/reports/final_summary.txt

Your report must include:

- Summary of findings  
- Errors discovered  
- Services status  
- Connectivity results  
- Resource usage summary  
- What you would fix if this were production  
- What additional tools or scripts you would implement  

---

# Deliverables

Your completed capstone should contain:


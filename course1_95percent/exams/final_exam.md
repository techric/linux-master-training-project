# Course 1 Final Exam: The 95% Linux Commands Certification

## Overview

This final exam evaluates your mastery of the essential Linux skills covered in Modules 01–09. These questions simulate the type of practical knowledge expected from a junior cloud engineer, cloud developer, or Linux administrator.

You may answer these questions using your own system. Document each answer clearly and include the command you used where applicable.

---

## Section 1: Navigation and Filesystem (Modules 01–02)

**1.1**  
What command shows your current working directory?

**1.2**  
Explain the difference between an absolute path and a relative path.  
Provide an example of each.

**1.3**  
List all files (including hidden files) in long format in your home directory.  
Provide the command you used.

**1.4**  
Navigate into `/etc`, list the contents, then return to your home directory.  
Show the commands used.

---

## Section 2: Permissions and Ownership (Module 03)

**2.1**  
Interpret the following permission string:

    drwxr-x---

Explain:

- File type  
- Owner permissions  
- Group permissions  
- Other permissions  

**2.2**  
Write the command to give the owner read/write/execute, the group read/execute, and deny all permissions to others.

**2.3**  
Change the owner of `/var/www/app` to user `ubuntu` and group `www-data`.

**2.4**  
A script named `deploy.sh` exists but will not run.  
Explain why this might happen and show how to fix it.

---

## Section 3: systemd and Services (Module 04)

**3.1**  
Check the status of the SSH service. Provide the command.

**3.2**  
Restart the `nginx` service. Provide the exact command.

**3.3**  
Explain the difference between:

- `systemctl restart nginx`  
- `systemctl reload nginx`

**3.4**  
Show how to enable a service to start on boot and how to disable it.

---

## Section 4: Logs and journalctl (Module 06)

**4.1**  
View the last 30 lines of the main system log. Provide the command.

**4.2**  
Search `/var/log/auth.log` for failed login attempts. Provide the command.

**4.3**  
Explain the purpose of each:

- `journalctl -u sshd`  
- `journalctl -b`  

**4.4**  
What does the following command do?

    journalctl -u nginx -f

---

## Section 5: Monitoring and Performance (Module 07)

**5.1**  
Show the top 10 processes by memory usage.

**5.2**  
Check total disk usage on the system in human-readable format.

**5.3**  
What does the `load average` represent in the output of `uptime`?

**5.4**  
View all open listening ports with process information. Provide the command.

---

## Section 6: Networking Basics (Module 08)

**6.1**  
Display all network interfaces and their IP addresses.

**6.2**  
Show the system’s routing table. Provide the command.

**6.3**  
Test DNS resolution for `example.com` using `dig`.

**6.4**  
Explain the difference between:

- `ping 8.8.8.8`  
- `ping google.com`

**6.5**  
Retrieve only the HTTP headers from `https://www.google.com`.

---

## Section 7: Shell Scripting (Module 09)

**7.1**  
What does the shebang line do in a shell script?

**7.2**  
Create a script named `info.sh` that prints the current date and username.  
Write the contents of the script.

**7.3**  
What do `$1`, `$2`, and `$#` represent in a shell script?

**7.4**  
Write a script snippet that checks if a file exists, and prints:

    File exists

otherwise prints:

    File not found

**7.5**  
Explain what an exit code is and what an exit code of `0` means.

---

## Section 8: Real-World Scenarios (Integrated Knowledge)

**8.1**  
You deploy an application but receive “Permission denied.”  
List three steps you would take to diagnose the issue.

**8.2**  
Your server cannot reach the internet. Give commands you would run to diagnose:

- Interface status  
- Routing  
- DNS  
- Connectivity  

**8.3**  
Nginx is installed, but accessing the server on port 80 fails.  
List at least five diagnostic steps involving systemd, firewall rules, and networking.

**8.4**  
You need to find the largest files on the system because disk space is running low.  
Provide the commands you would use.

---

## Submission Instructions

Submit your answers in a document named:

    final_exam_answers.txt

Include:

- Commands you used  
- Explanations in your own words  
- Any issues encountered  

Screenshots are optional, but can help if you are using this exam for portfolio or review.

---

## Completion Criteria

You have completed this final exam when:

- All questions are answered clearly and completely  
- Commands are correct and demonstrate understanding  
- Explanations show mastery of the 95% Linux skillset  
- You can troubleshoot scenarios without guessing  

---

## Notes and Personal Observations

Use this section to reflect on:

- Areas that felt easy  
- Topics requiring more practice  
- Real-world relevance of each section  
- Personal improvements since Module 01  

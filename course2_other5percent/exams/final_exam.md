# Final Exam: Advanced Linux Administration, Diagnostics, and Incident Response  
(The Other 5%)

## Overview

This final exam evaluates mastery of high-level Linux skills across:

- Process debugging  
- Kernel-level diagnostics  
- Advanced networking analysis  
- Storage and filesystem recovery  
- Security and hardening  
- Incident response  
- Forensics and intrusion detection  
- Dependency and package chain troubleshooting  

The exam reflects real-world, senior-level cloud engineering and Linux administration challenges.  
All answers should be written as if you were documenting findings for a production incident or technical leadership.

---

# Instructions

- Answer all questions thoroughly.  
- Show commands you would use.  
- Explain *why* each command or technique is appropriate.  
- When in doubt, provide more detail—not less.  
- This exam assumes root or sudo-level access unless noted otherwise.  

Total Questions: **40**  
Passing Level: **Mastery (90%+)**

---

# Section 1 — Process Debugging & System Call Tracing (10 Questions)

1. A process is hung and consuming 100% CPU. Describe the exact sequence of commands you would run to diagnose the root cause, and explain the purpose of each.  
2. What does `strace -p PID -e trace=file` reveal? Provide a real-world scenario where this option identifies the actual problem.  
3. How do you identify processes running from deleted binaries? Why is this a common sign of compromise?  
4. What command shows a full stack trace of a running process, and what information can be gathered from it?  
5. How would you use `ps -eo` to identify potential parent-child process anomalies?  
6. Describe how to determine whether a hung process is waiting on I/O, network, CPU, or locks.  
7. Explain how you would use `lsof` to determine if a process is holding open a deleted log file and explain the impact this can have on disk usage.  
8. A process appears normal in `ps`, but behaves erratically. Outline how you would diagnose it using `strace`, `lsof`, and kernel logs.  
9. How do you detect hidden processes by comparing `/proc` entries against `ps` output?  
10. Explain the difference between debugging a CPU-bound hang vs. a syscall-blocked hang.

---

# Section 2 — Advanced Networking & Socket-Level Diagnostics (6 Questions)

11. What is the difference between `ss -tulpn` and `lsof -i` when diagnosing network issues?  
12. How do you trace a program’s DNS lookups in real time?  
13. Given repeated “connection reset” issues, describe how to analyze kernel logs and socket states to determine whether the root cause is network, application, or OS-level.  
14. How do you correlate application timeouts with NIC resets recorded in `dmesg`?  
15. What sequence of commands helps identify a reverse shell in progress?  
16. Explain how to detect a process that is listening on a port but no longer has a valid binary on disk.

---

# Section 3 — Storage, Filesystem, and I/O Diagnostics (6 Questions)

17. A filesystem remounts as read-only during peak traffic. Explain how to diagnose the cause using `dmesg`, SMART data, and fsck.  
18. How do inode exhaustion and disk space exhaustion differ? Provide symptoms and detection commands.  
19. Describe how to detect failing disks using SMART testing and kernel logs.  
20. How do you identify which process is generating heavy disk writes in real time?  
21. An application fails because a shared library is missing. Explain how to identify the cause using `ldd`, `ldconfig`, and package metadata.  
22. Explain how to inspect an LVM volume group that fails activation during boot.

---

# Section 4 — Security, Intrusion Detection, and Hardening (10 Questions)

23. Explain how to detect unauthorized SSH logins using `/var/log/auth.log` and journald filters.  
24. Describe how to identify brute-force attempts and extract the top attacking IPs.  
25. A suspicious systemd service appears on a production server. Describe how to analyze everything about that service, including its origin.  
26. Explain how to review all persistence mechanisms an attacker may have installed.  
27. Identify three signs of privilege escalation in Linux logs.  
28. Explain the difference between SELinux and AppArmor denial types and how to debug each.  
29. What is the security risk of setuid binaries, and how do you audit them?  
30. Provide a sequence of commands to detect malicious outbound traffic from a compromised host.  
31. Describe how to analyze unauthorized modifications to `/etc/sudoers` and `/etc/sudoers.d/`.  
32. Explain how kernel-level restrictions (ptrace, ASLR, suid_dumpable) can improve security.

---

# Section 5 — Incident Response, Forensics & Post-Mortem (8 Questions)

33. Describe the full chain of actions you must take BEFORE rebooting a compromised Linux system.  
34. Explain how to perform a timeline analysis using filesystem metadata and logs.  
35. How do you identify malware running from memory only?  
36. Explain why disk imaging with `dd` must be done with read-only mounting afterward.  
37. An attacker replaced binaries in `/usr/local/bin`. Describe how to detect this.  
38. Provide a high-level remediation and containment plan for a compromised cloud instance.  
39. After resolving the incident, outline what must be included in the root-cause analysis and final incident report.  
40. Explain why “rebuild from a golden image” is often safer than attempting to repair a compromised system manually.

---

# End of Final Exam

# Cheat Sheet: Advanced Linux (The Other 5%)  
High-Severity, Low-Frequency Diagnostics for Senior Cloud Engineers

---

# 1. Advanced Process Debugging

## Identify Hung / Suspicious Processes
    ps -eo pid,ppid,user,%cpu,%mem,cmd --sort=-%cpu
    ps aux | grep defunct
    ls -l /proc/*/exe | grep deleted

## System Call Tracing
    strace -p PID
    strace -p PID -e trace=file
    strace -p PID -e trace=network
    strace -f -o trace.log /path/to/binary

## Stack Traces
    sudo pstack PID
    sudo gdb -p PID -ex bt -batch

## Open Files & Sockets
    sudo lsof -p PID
    sudo lsof | grep deleted

---

# 2. Advanced Networking Diagnostics

## List All Active Connections
    ss -tulpn
    ss -anp | grep ESTAB

## Detect Reverse Shell
    ss -tp | grep -v "127.0.0.1"
    ps aux | grep "bash -i"
    lsof -i

## DNS / Network Syscalls
    strace -p PID -e trace=network

## Kernel NIC Issues
    dmesg | grep -i eth
    dmesg | grep -i net

---

# 3. Storage, Filesystem, and I/O Recovery

## Block Devices and Filesystems
    lsblk -f
    blkid
    fdisk -l

## Detect or Fix Corruption
    dmesg | grep -i error
    sudo fsck -f /dev/sdX#

## SMART Disk Checks
    sudo smartctl -H /dev/sda
    sudo smartctl -a /dev/sda

## I/O Performance Diagnostics
    iostat -xz 1
    sudo iotop

## Recover Deleted File (If Still Open)
    cp /proc/PID/fd/FD_NUMBER /root/recovered

---

# 4. Security, Hardening, and Intrusion Detection

## Authentication Analysis
    last
    lastb
    sudo egrep "Failed|Accepted" /var/log/auth.log

## Brute Force IP Extraction
    sudo grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr

## Suspicious Services & Persistence
    sudo systemctl list-unit-files --type=service
    sudo crontab -l
    sudo ls /etc/cron.*
    cat ~/.ssh/authorized_keys

## Setuid & Capabilities
    sudo find / -perm -4000 -type f
    sudo getcap -r / 2>/dev/null

## SELinux / AppArmor Diagnostics
    getenforce
    sudo ausearch -m avc
    sudo sealert -a /var/log/audit/audit.log

---

# 5. Logging, Kernel Tracing, Boot Diagnostics

## Advanced journalctl
    journalctl --since "10 minutes ago"
    journalctl -u service -b
    journalctl -o json-pretty

## Kernel Messages
    dmesg --follow
    dmesg | grep -i oom
    dmesg | grep -i blk

## Boot / Startup Analysis
    systemd-analyze
    systemd-analyze blame
    systemd-analyze critical-chain

---

# 6. Package & Dependency Failures

## Debian/Ubuntu
    dpkg -l
    dpkg -L packagename
    apt-cache depends packagename
    apt --fix-broken install

## RHEL/CentOS
    rpm -qa
    rpm -ql packagename
    repoquery --requires packagename

## Libraries
    ldd /path/to/binary
    ldconfig -p | grep lib

---

# 7. Incident Response Workflow (Condensed)

## 1. Isolate System
    iptables -P INPUT DROP
    iptables -P OUTPUT DROP

## 2. Capture Volatile Evidence
    ps aux
    sudo ss -tulpn
    ls -l /proc/*/exe | grep deleted
    sudo gcore -o /root/memdump PID
    dmesg | tail -100

## 3. Identify IOCs
- Suspicious services  
- Cron jobs  
- Unauthorized SSH keys  
- Outbound connections  

## 4. Timeline Reconstruction
    journalctl --since "..." --until "..."
    sudo egrep "Failed|Accepted" /var/log/auth.log

## 5. Disk Image (Forensic)
    sudo dd if=/dev/sda of=/root/forensic.img bs=4M status=progress
    sha256sum forensic.img

## 6. Remediate
- Remove persistence  
- Patch packages  
- Rebuild from golden image  
- Rotate credentials  

---

# 8. Quick Reference Tables

## Key Tools Summary

| Tool       | Purpose                               |
|------------|----------------------------------------|
| strace     | System call tracing                    |
| lsof       | Open files, sockets, locks             |
| pstack     | Stack traces                           |
| gdb        | Advanced debugging                     |
| ss         | Socket-level diagnostics               |
| iostat     | I/O performance                        |
| auditd     | Security auditing framework            |
| smartctl   | Disk hardware diagnostics              |
| sealert    | SELinux debugging                      |
| repoquery  | Package dependency analysis            |

---

# 9. Indicators of Compromise (IOC) Checklist

- Unknown systemd unit  
- Cron jobs created recently  
- SSH keys added without authorization  
- Processes running from deleted binaries  
- Repeated login failures from foreign IPs  
- Unexpected outbound connections  
- Modified PATH directories  
- Modified /etc/sudoers or sudoers.d rules  
- Files modified recently in sensitive directories  
- Binaries with unusual permissions (setuid)  

---

# End of Cheat Sheet

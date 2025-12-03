# Linux 95% Commands Cheatsheet  
Essential Commands for Cloud Engineers, Cloud Developers, and Linux Administrators

This cheatsheet summarizes the core commands covered in Modules 01–09.  
These commands solve more than 95% of real-world Linux workflow and troubleshooting tasks.

---

# 1. Basic Navigation

Show current directory:
    pwd

List files:
    ls

List all files (including hidden):
    ls -a

Long listing:
    ls -l

All + long list:
    ls -la

Change directory:
    cd /path/to/location

Go up one directory:
    cd ..

Go to home:
    cd ~

---

# 2. File and Directory Management

View file:
    cat filename

Page through file:
    less filename

Copy file:
    cp source destination

Move or rename:
    mv source destination

Remove file:
    rm filename

Remove directory recursively:
    rm -r dirname

Create directory:
    mkdir dirname

Create nested directories:
    mkdir -p path/to/create

---

# 3. Permissions and Ownership

View permissions:
    ls -l

Change permissions (numeric):
    chmod 755 filename

Change permissions (symbolic):
    chmod u+rwx,g+rx,o-r filename

Change owner:
    chown user filename

Change owner and group:
    chown user:group filename

---

# 4. Searching and Finding Files

Search inside a file:
    grep "text" filename

Recursive search:
    grep -r "text" /path

Find a file by name:
    find /path -name filename

Find large files:
    find / -type f -size +100M

---

# 5. systemd and Services

Check service status:
    systemctl status service_name

Start service:
    systemctl start service_name

Stop service:
    systemctl stop service_name

Restart service:
    systemctl restart service_name

Reload service:
    systemctl reload service_name

Enable service at boot:
    systemctl enable service_name

Disable service:
    systemctl disable service_name

---

# 6. Logs and journalctl

Logs for a service:
    journalctl -u service_name

Logs since last boot:
    journalctl -b

Follow logs:
    journalctl -u service_name -f

Tail log files:
    tail -n 50 /var/log/syslog

Search logs:
    grep "error" /var/log/auth.log

---

# 7. Monitoring and Performance

System usage:
    top

Better process viewer (if installed):
    htop

Memory usage:
    free -h

Disk usage by filesystem:
    df -h

Disk usage inside a directory:
    du -h /path

Sorted largest directories:
    du -ah / | sort -h | tail

Load average:
    uptime

Open listening ports:
    ss -tulnp

---

# 8. Networking Essentials

Show interfaces:
    ip addr

Brief interface summary:
    ip -brief addr

Routing table:
    ip route

Ping test:
    ping hostname_or_ip

DNS test:
    dig example.com

Hostname lookup:
    nslookup example.com

Get HTTP headers only:
    curl -I https://example.com

---

# 9. Process Management

All processes:
    ps aux

Search for a process:
    ps aux | grep processname

Kill a process:
    kill PID

Force kill:
    kill -9 PID

---

# 10. Archives and Compression

Create tar archive:
    tar -cvf archive.tar /path

Extract tar:
    tar -xvf archive.tar

Extract tar.gz:
    tar -xzvf file.tar.gz

Compress file:
    gzip filename

Decompress:
    gunzip filename.gz

---

# 11. Package Management

Debian/Ubuntu:
    sudo apt update
    sudo apt install package
    sudo apt remove package

RHEL/CentOS/Amazon Linux:
    sudo yum install package
    sudo yum remove package

RPM query:
    rpm -qa | grep package

---

# 12. Shell Scripting Basics

Shebang:
    #!/bin/bash

Variables:
    name="Eric"
    echo "$name"

If statement:
    if [ -f filename ]; then
        echo "File exists"
    else
        echo "File not found"
    fi

Loop:
    for i in 1 2 3 4 5; do
        echo "$i"
    done

Exit code:
    echo $?

---

# Quick Memory Reference

Navigation: ls, cd, pwd  
File operations: cp, mv, rm, mkdir  
Permissions: chmod, chown  
Search: grep, find  
Services: systemctl  
Logs: journalctl, /var/log/*  
Monitoring: df, du, top, free  
Networking: ip, ping, dig, curl  
Archives: tar, gzip  
Scripting: shebang, variables, if statements, loops  

---

# End of Cheatsheet

# Module 07: Advanced Package Management, Dependency Analysis, and Build Troubleshooting  
(The Other 5%)

## Overview

This module teaches advanced package management and dependency resolution techniques. These are the tools you reach for when:

- Packages conflict with each other  
- Dependency trees become broken  
- Updates fail due to version lock or architecture mismatch  
- A library upgrade breaks an application  
- You must build or inspect software from source  
- You must pin or freeze specific package versions  
- You must roll back or downgrade packages safely  

This level of package troubleshooting is not part of daily cloud engineering—but when things break, this knowledge becomes essential.

---

## Learning Objectives

By the end of this module, you should be able to:

- Inspect dependency trees and reverse dependencies  
- Diagnose broken package states  
- Identify version conflicts and unmet dependencies  
- Use dpkg and rpm for low-level package inspection  
- Pin or lock versions to stabilize environments  
- Diagnose shared library issues using ldd  
- Inspect build behavior using source files and system headers  
- Rebuild packages or libraries when necessary  

---

## Lesson 1: Low-Level Package Information (dpkg, rpm)

### Debian/Ubuntu dpkg

List installed packages:
    dpkg -l

Show details for one package:
    dpkg -s packagename

List all files installed by a package:
    dpkg -L packagename

Check which package owns a file:
    dpkg -S /usr/bin/python3

Install a .deb manually:
    sudo dpkg -i package.deb

### RHEL/CentOS/Amazon rpm

List installed packages:
    rpm -qa

Show package metadata:
    rpm -qi packagename

List files installed:
    rpm -ql packagename

Find the owner of a file:
    rpm -qf /usr/bin/python3

Install an RPM:
    sudo rpm -ivh something.rpm

These commands reveal the internal structure of packages and their installed files.

---

## Lesson 2: Dependency Trees and Reverse Dependencies

### Debian-based systems

Show dependencies:
    apt-cache depends packagename

Show reverse dependencies:
    apt-cache rdepends packagename

Check if a version upgrade is possible:
    apt-cache policy packagename

### RHEL-based systems

Dependencies:
    repoquery --requires packagename

Reverse dependencies:
    repoquery --whatrequires packagename

Package dependency analysis is essential when upgrading critical components (OpenSSL, Python, systemd, etc.).

---

## Lesson 3: Diagnosing Broken Package States

When apt fails with dependency errors:

    sudo apt --fix-broken install

When dpkg is partially installed:

    sudo dpkg --configure -a

Remove a package with broken dependencies:
    sudo dpkg --remove --force-remove-reinstreq packagename

Force overwrite collisions:
    sudo dpkg -i --force-overwrite package.deb

Check apt logs:
    cat /var/log/apt/term.log

Broken states often occur after:

- Manual installation of mismatched versions  
- Interrupted updates  
- Mixed repositories (e.g., Ubuntu + Debian sources)  

---

## Lesson 4: Version Pinning, Holding, and Freezing

Prevent a package from updating:
    sudo apt-mark hold packagename

Undo hold:
    sudo apt-mark unhold packagename

Pin package to a version:
    sudo nano /etc/apt/preferences.d/custom

Example:
    Package: nginx
    Pin: version 1.18.*
    Pin-Priority: 1001

In RHEL-based systems, use versionlock:

    sudo yum install yum-plugin-versionlock
    sudo yum versionlock packagename-1.2.3-1

Used when an application depends on a specific library version and cannot survive automatic upgrades.

---

## Lesson 5: Troubleshooting Shared Library Issues (ldd, ldconfig)

Check linked libraries:
    ldd /usr/bin/nginx

Look for “not found” entries—common after library updates.

Update system library cache:
    sudo ldconfig

Search for missing libraries:
    ldconfig -p | grep libssl

Trace library loading (advanced):
    strace -e trace=openat nginx

Library mismatches break applications written in:

- Python  
- Ruby  
- Node  
- C / C++  
- Go (sometimes)  

When something fails without clear logs, shared libraries are often the cause.

---

## Lesson 6: Building From Source (Advanced)

Install build dependencies:
    sudo apt build-dep packagename

Clone source:
    git clone https://github.com/project/repo.git

Build:
    ./configure
    make
    sudo make install

Or using modern build systems:

    cmake .
    make

Inspect system include files:
    ls /usr/include/

Inspect compilation flags:
    gcc -v

Source-level troubleshooting is rare but essential when:

- A cloud application requires custom patches  
- A package version is too old  
- Dependency chains conflict with system libraries  

---

## Lesson 7: Detecting and Fixing Repository Issues

List configured sources:

Debian:
    cat /etc/apt/sources.list
    ls /etc/apt/sources.list.d/

RHEL:
    cat /etc/yum.repos.d/*.repo

Invalidate corrupted caches:
    sudo apt clean
    sudo yum clean all

Refresh metadata:
    sudo apt update
    sudo yum makecache

Repository problems are common in:

- Self-hosted package mirrors  
- Air-gapped environments  
- Hybrid Debian/RHEL systems  
- Old EC2 AMIs  

---

## Lesson 8: Real-World Scenarios

### Scenario 1: Application fails after system update

Symptom: “libxyz.so.1 not found”

1. Run ldd:
       ldd /usr/bin/myapp

2. Search for the library:
       ldconfig -p | grep libxyz

3. Identify missing or incompatible version
4. Reinstall or downgrade library

---

### Scenario 2: Upgrade fails due to dependency conflict

1. View dependency graph:
       apt-cache depends packagename

2. Look for pinned versions:
       apt-mark showhold

3. Run fix:
       sudo apt --fix-broken install

---

### Scenario 3: Building software requires missing headers

1. Check include folders:
       ls /usr/include

2. Install -dev packages:
       sudo apt install libssl-dev

3. Confirm:
       gcc -v

---

## Lab Exercise

1. Show what depends on libssl:
       apt-cache rdepends libssl

2. Inspect shared libraries for a binary:
       ldd /bin/ls

3. Pin a package:
       apt-mark hold nginx

4. Break and repair dependency state (in a VM):
       sudo dpkg --remove --force-remove-reinstreq somepackage
       sudo apt --fix-broken install

5. Build a small C program:
       echo 'int main(){return 0;}' > test.c
       gcc test.c -o test

Document any surprises.

---

## Quiz

1. What is the difference between dpkg and apt?  
2. What does `apt-mark hold` do?  
3. How do you discover reverse dependencies on Debian?  
4. When should you use ldd?  
5. What are signs of a corrupted package repository?  

---

## Notes and Observations

Use this space to record:

- Dependency loops you discovered  
- Library mismatches  
- Build failures and solutions  
- Repository issues  
- Version-pin strategies you prefer  

# Module 05: Installing and Managing Software Packages

## Overview

This module introduces package management in Linux. Package managers allow you to install, remove, update, and search for software. As a cloud engineer or developer, you will constantly use package managers to install dependencies, update servers, and troubleshoot failed deployments.

Different Linux distributions use different managers, but the concepts remain consistent.

## Learning Objectives

By the end of this module, you should be able to:

- Understand what a package manager does  
- Install, update, and remove software packages  
- Use `apt`, `yum`, or `dnf` depending on the distribution  
- Search for packages and inspect their details  
- Update system repositories and understand package sources  
- Troubleshoot common installation issues  

---

## Lesson 1: Understanding Package Managers

Linux distributions use different package managers:

- **apt** — Ubuntu, Debian  
- **yum** — older CentOS, older Amazon Linux  
- **dnf** — newer Fedora, CentOS Stream, Amazon Linux 2023  

Package managers handle:

- Installation  
- Removal  
- Updates  
- Dependency resolution  
- Downloading and verifying packages  

Packages come from repositories managed by the distribution.

---

## Lesson 2: Using apt (Ubuntu, Debian)

Update the package list:

    sudo apt update

Upgrade installed packages:

    sudo apt upgrade

Install a package:

    sudo apt install nginx

Remove a package:

    sudo apt remove nginx

Remove package + config files:

    sudo apt purge nginx

Search for a package:

    apt search docker

View details about a package:

    apt show nginx

---

## Lesson 3: Using yum (Amazon Linux 2, older CentOS)

Install a package:

    sudo yum install httpd

Remove a package:

    sudo yum remove httpd

Update a package:

    sudo yum update httpd

Update all packages:

    sudo yum update -y

Search for a package:

    yum search python3

Get details:

    yum info httpd

List installed packages:

    yum list installed

---

## Lesson 4: Using dnf (Amazon Linux 2023, CentOS Stream, newer Fedora)

Install a package:

    sudo dnf install git

Remove a package:

    sudo dnf remove git

System upgrade:

    sudo dnf upgrade

Search for packages:

    dnf search nodejs

View package information:

    dnf info git

List installed packages:

    dnf list installed

---

## Lesson 5: Understanding Dependencies and Repositories

When installing software, package managers automatically:

- Install required dependencies  
- Resolve version conflicts  
- Verify packages using digital signatures  

To view enabled repositories:

    sudo yum repolist
    sudo dnf repolist
    sudo apt-cache policy

To enable optional repositories (example for Amazon Linux):

    sudo dnf config-manager --set-enabled extras

If packages are missing, they are often in:

- EPEL repository  
- Extras repository  
- Universe repository (Ubuntu)

---

## Lesson 6: Troubleshooting Package Installation

### Error: “Package not found”

Check repositories:

    dnf repolist
    yum repolist
    apt-cache policy

### Error: “Failed to download metadata”

Fix by clearing the cache:

    sudo dnf clean all
    sudo yum clean all
    sudo apt clean

### Error: Locked package manager

Only one process can use the package manager at a time.

Find the process:

    ps aux | grep apt
    ps aux | grep yum

Kill the process if safe.

### Error: Dependency conflicts

Often resolved by upgrading:

    sudo dnf upgrade
    sudo yum update -y
    sudo apt upgrade

---

## Lab Exercise

Perform the following tasks based on your Linux distribution:

1. Update your package list:

       sudo apt update
       sudo yum update -y
       sudo dnf upgrade

2. Install a package (choose something simple):

       sudo apt install tree
       sudo yum install tree
       sudo dnf install tree

3. Verify the installation:

       which tree
       tree --version

4. Search for another package:

       apt search htop
       yum search htop
       dnf search htop

5. Install it and then remove it:

       sudo apt install htop
       sudo yum install htop
       sudo dnf install htop

       sudo apt remove htop
       sudo yum remove htop
       sudo dnf remove htop

6. List installed packages and find anything unexpected.

---

## Quiz

1. What is a package manager and why does Linux use one?  
2. What does `sudo apt update` do?  
3. What is the difference between `yum` and `dnf`?  
4. How do you search for a package?  
5. What problems arise when two processes try to use a package manager at the same time?  

---

## Notes and Personal Observations

Record:

- Packages you installed  
- Differences you noticed between apt, yum, and dnf  
- Commands you want to remember  
- Any errors you encountered  
